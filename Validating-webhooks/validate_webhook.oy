from flask import Flask, request, jsonify
import base64
import logging

webhook_server = Flask(__name__)

webhook_server.config['ENV'] = "development"

webhook_server.logger.setLevel(logging.INFO)


@webhook_server.route('/detect', methods=['POST'])
def validating_webhook():
    request_info = request.get_json()
    print(request_info)
    uid = request_info["request"].get("uid")
    if request_info["request"]["object"]["metadata"]["labels"].get(webhook_server.config['ENV']):
        webhook_server.logger.info(f' Component {request_info["request"]["object"]["kind"]}/{request_info["request"]["object"]["metadata"]["name"]} has required values set while creation \"{webhook_server.config["ENV"]}\" label. So Accepted the request.')
        return admission_result(True, uid, f"{webhook_server.config['ENV']}  label is present.")
    else:
        webhook_server.logger.error(f' {request_info["request"]["object"]["metadata"]["name"]}/{request_info["request"]["object"]["kind"]} does not have required \"{webhook_server.config["ENV"]}" labels, so  Request Rejected! ')
        return admission_result(False, uid, f" The Labels \"{webhook_server.config['ENV']}\" is not set while creating object")

def admission_result(allowed, uid, message):
    return jsonify({"apiVersion": "admission.k8s.io/v1",
                    "kind": "AdmissionReview",
                    "response":
                        {"allowed": allowed,
                         "uid": uid,
                         "status": {"message": message}
                        }
                    })

if __name__ == '__main__':
    webhook_server.run(host='0.0.0.0', port=5000, ssl_context=("webhook.crt", "webhook.key"))

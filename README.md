from mongoengine import *

connect('railway', host='localhost', port=27017)

class Train(EmbeddedDocument):
    train_id = StringField(required=True)
    location = StringField(required=True)
    speed = FloatField(required=True)
    status = StringField(required=True)


(link unavailable)

from mongoengine import *

class Section(EmbeddedDocument):
    section_id = StringField(required=True)
    status = StringField(required=True)
    occupancy = BooleanField(required=True)


(link unavailable)

from mongoengine import *

class Alert(EmbeddedDocument):
    alert_id = StringField(required=True)
    message = StringField(required=True)
    location = StringField(required=True)


(link unavailable)

from flask import Blueprint, request, jsonify
from train_model import Train

train_blueprint = Blueprint('train', __name__)

@train_blueprint.route('/trains', methods=['GET'])
def get_trains():
    trains = Train.objects()
    return jsonify([train.to_json() for train in trains])

@train_blueprint.route('/trains/<string:train_id>', methods=['GET'])
def get_train(train_id):
    train = Train.objects(train_id=train_id).first()
    return jsonify(train.to_json())

@train_blueprint.route('/trains', methods=['POST'])
def create_train():
    train = Train(**request.get_json())
    train.save()
    return jsonify(train.to_json())

@train_blueprint.route('/trains/<string:train_id>', methods=['PUT'])
def update_train(train_id):
    train = Train.objects(train_id=train_id).first()
    train.update(**request.get_json())
    return jsonify(train.to_json())


(link unavailable)

from flask import Blueprint, request, jsonify
from section_model import Section

section_blueprint = Blueprint('section', __name__)

@section_blueprint.route('/sections', methods=['GET'])
def get_sections():
    sections = Section.objects()
    return jsonify([section.to_json() for section in sections])

@section_blueprint.route('/sections/<string:section_id>', methods=['GET'])
def get_section(section_id):
    section = Section.objects(section_id=section_id).first()
    return jsonify(section.to_json())

@section_blueprint.route('/sections', methods=['POST'])
def create_section():
    section = Section(**request.get_json())
    section.save()
    return jsonify(section.to_json())

@section_blueprint.route('/sections/<string:section_id>', methods=['PUT'])
def update_section(section_id):
    section = Section.objects(section_id=section_id).first()
    section.update(**request.get_json())
    return jsonify(section.to_json())


(link unavailable)

from flask import Blueprint, request, jsonify
from alert_model import Alert

alert_blueprint = Blueprint('alert', __name__)

@alert_blueprint.route('/alerts', methods=['GET'])
def get_alerts():
    alerts = Alert.objects()
    return jsonify([alert.to_json() for alert in alerts])

@alert_blueprint.route('/alerts/<string:alert_id>', methods=['GET'])
def get_alert(alert_id):
    alert = Alert.objects(alert_id=alert_id).first()
    return jsonify(alert.to_json())

@alert_blueprint.route('/alerts', methods=['POST'])
def create_alert():
    alert = Alert(**request.get_json())
    alert.save()
    return jsonify(alert.to_json())

@alert_blueprint.route('/alerts/<string:alert_id>', methods=['PUT'])
def update_alert(alert_id):
    alert = Alert.objects(alert_id=alert_id).first()
    alert.update(**request.get_json())
    return jsonify(alert.to_json())


(link unavailable)

from flask import Flask
from train_controller import train_blueprint
from section_controller import section_blueprint
from alert_controller import alert_blueprint

app = Flask(__name__)

app.register_blueprint(train_blueprint, url_prefix='/api')
app.register_blueprint(section_blueprint, url_prefix='/api')
app.register_blueprint(alert_blueprint, url_prefix='/api')

if __name__ == '__main__':
    app.run(debug=True)

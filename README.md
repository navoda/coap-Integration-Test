# Coap-Integration-Test
Created for the purpose of storing test components of Coap-integration

## Components 

### WSO2 CoAP Input Adapter
[carbon-device-mgt-plugins](https://github.com/navoda/carbon-device-mgt-plugins/tree/coap-integration-release-3.0.x/components/extensions/cdmf-transport-adapters/input/org.wso2.carbon.device.mgt.input.adapter.coap) - `branch coap-integration-release-3.0.x`

The basic component of the integration. It is a component of WSO2 input adapters and also a trasportation interface, created to communicate with WSO2 IoT server via [CoAP (Constrained Application Protocol)](https://tools.ietf.org/html/rfc7252). This interface implementation, is based on CoRE [RD (Resource Directory)](https://tools.ietf.org/html/draft-ietf-core-resource-directory-08) concept which register the basic WSO2 IoT device API architecture and allows to send REST messages to/from enrolled devices in WSO2 IoT server. 

### Modified WSO2 Webapp-Publisher
[carbon-device-mgt](https://github.com/navoda/carbon-device-mgt/tree/coap-integration-2.0.x/components/apimgt-extensions/org.wso2.carbon.apimgt.webapp.publisher) - `coap-integration-2.0.x`

This modification is used to send register requests from every device API in device-plugin to CoAP RD (Resource Directory) during the server startup. A **Resource Directory Client** is implemented to send these requests. 

### Californium Libraries
1. [californium-core](https://github.com/eclipse/californium-core)
2. [californium-proxy](https://github.com/eclipse/californium-proxy)
3. [californium.tools](https://github.com/navoda/californium.tools/tree/modifyRDNodeResource) - `branch modifyRDNodeResource`

## Initialization instructions

### Copper(Cu)
[Copper](http://people.inf.ethz.ch/mkovatsc/copper.php) is a firefox plugin, which used as a CoAP end-client. install it into fiefox using this link [https://addons.mozilla.org/en-US/firefox/addon/copper-270430/](https://addons.mozilla.org/en-US/firefox/addon/copper-270430/)

### WSO2 Server
Before starting the WSO2 IoT server please add mentioned files into following locations.

#### dropins 
(core/repository/components/dropins)
[coap-input-adapter](https://github.com/navoda/coap-Integration-Test/blob/master/org.wso2.carbon.device.mgt.input.adapter.coap-3.0.2-SNAPSHOT.jar)

#### patches
(core/repository/components/patches)
[webapp-publisher](https://github.com/navoda/coap-Integration-Test/blob/master/org.wso2.carbon.apimgt.webapp.publisher-2.0.2-SNAPSHOT.jar)
 
#### lib
(core/repository/components/lib)

1. [cf-rd](https://github.com/navoda/coap-Integration-Test/blob/master/cf-rd-1.1.0-SNAPSHOT.jar)
2. [cf-proxy](https://github.com/navoda/coap-Integration-Test/blob/master/californium_proxy_1.1.0_SNAPSHOT_1.0.0.jar)
3. [jackson-core](https://github.com/navoda/coap-Integration-Test/blob/master/jackson-core-asl-1.9.0.jar)
4. [jackson-mapper](https://github.com/navoda/coap-Integration-Test/blob/master/jackson-mapper-asl-1.9.0.jar)

## Test
Run _broker/wso2server.sh_ and _core/wso2server.sh_

Open **Copper (Cu)** firefox plugin in coap default port **5683** and click `Discover` (or `GET` the _well-known-core_). Once the content finished download it will show the tree view of current **RD**(Resource Directory) with WSO2 device APIs

### REST Methods
Hover the mouse pointer on the **end Resource** to see the type of method `GET/POST/PUT/DELETE`is allowed to pass.
_**Important: Even a resource only allowed a `GET` method, it is recommended to send a `POST` for that, if a header should be attached in the message**_ 

### Set URI
click the end resource to get the **URI** to the URI-bar. Before sending it, make sure to set appropriate values in the locations of dynamic resources.

E.g: if the _**deviceId**_ of a raspberrypi device is `uvgzi62oz2go`, set the `{deviceId}` in `coap://localhost:5683/rd/raspberrypi/device/{deviceId}/bulb`  as `coap://localhost:5683/rd/raspberrypi/device/uvgzi62oz2go/bulb` 

### Set Message
use the outgoing tab to write a message.
If specific headers and body should be attached, use following _json_ format to write a message

```json
{"header":{"header_title":"value"},"body":"body_text"}
```

### Send Message
Send the messgae using appropriate REST method buttons. [see Copper(Cu) usage](http://people.inf.ethz.ch/mkovatsc/copper.php/usage)

### virtual-firealarm Example

1. Before starting the server, replace the virtual_firealarm.war file of virtual_firealarm webapp folder in (core/repository/components/features) with the [virtual_firealarm.war](https://github.com/navoda/coap-Integration-Test/blob/master/virtual_firealarm.war) in this repository.
2. Follow the above initialization instructions and start the servers.
3. Enroll a new virtual-firealarm from [https://localhost:9443/devicemgt](https://localhost:9443/devicemgt). Downlaod and start the agent.
4. Start Copper(Cu) and get the wel_known_core
5. Get the device_id of the firealarm and access token.
6. Click the virtual-firealarm `buzz` end-Resource and get `coap://localhost:5683/rd/virtual_firealarm/device/{deviceId}/buzz` to the URI bar
7. set the outgoing message to (if device_id is `uvgzi62oz2go` and access token is `93392003-746e-3fbd-aa79-1b2ed7a97508`)
```json
{ 
  "header":{
     "Authorization":"Bearer 93392003-746e-3fbd-aa79-1b2ed7a97508",
    "Content-Type":"application/x-www-form-urlencoded"
  },
   "body":"state=on"
}
```
8. Send the message as a `POST`
9. Check if the buzzer changed its' state to 'on'.

```
Req: POST coap://localhost:5683/rd/virtual_firealarm/device/uvgzi62oz2go/buzz
Payload: {"header":{"Authorization":"Bearer 93392003-746e-3fbd-aa79-1b2ed7a97508","Content-Type":"application/x-www-form-urlencoded" },"body":"state=on"}
Content-Format: application/json

Res: ACK 2.05 Content
Location: http://localhost:9763/virtual_firealarm/device/uvgzi62oz2go/buzz
```

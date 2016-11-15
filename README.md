# Coap-Integration-Test
Created for the purpose of storing test components of Coap-protocol-integration

### components
1. [carbon-device-mgt-plugins](https://github.com/navoda/carbon-device-mgt-plugins/tree/coap-protocol-integration) - `branch coap-protocol-integration`
2. [carbon-device-mgt](https://github.com/navoda/carbon-device-mgt/tree/coap-v1.1.0) - `branch coap-v1.1.0`
3. [californium.tools](https://github.com/navoda/californium.tools/tree/modifyRDNodeResource) - `branch modifyRDNodeResource`
4. [californium](https://github.com/eclipse/californium) - `master`

## Initialization instructions

1. use `wso2iots-1.0.0-alpha` as the server
2. `dropins` -  [org.wso2.carbon.device.mgt.iot.input.adapter.coap-2.1.3-SNAPSHOT.jar](https://github.com/navoda/coap-Integration-Test/blob/master/org.wso2.carbon.device.mgt.iot.input.adapter.coap-2.1.3-SNAPSHOT.jar)
3. `patches` - [org.wso2.carbon.apimgt.webapp.publisher-1.1.0.jar](https://github.com/navoda/coap-Integration-Test/blob/master/org.wso2.carbon.apimgt.webapp.publisher-1.1.0.jar)
4. `lib` - [californium_proxy_1.1.0_SNAPSHOT_1.0.0.jar] (https://github.com/navoda/coap-Integration-Test/blob/master/californium_proxy_1.1.0_SNAPSHOT_1.0.0.jar) and [cf-rd-1.1.0-SNAPSHOT.jar](https://github.com/navoda/coap-Integration-Test/blob/master/cf-rd-1.1.0-SNAPSHOT.jar)
5. run wso2server.sh
6. open **Copper (Cu)** firefox plugin and `GET` the _well-known-core_. Once the content finished download it will show the tree view of current **RD**(Resource Directory)

## Send a Request
**At the moment only `GET` requests are properly handled and those requests must be send as `POST`** _because `GET` dosen't allow a authentication payload (will figure some alternative way)_
Eg: 

1. Send a `POST` request to `rd/raspberrypi/1.0.0/device/stats/{deviceId}` with a value to {deviceId} (originaly a `GET` request) 
2. Set payload as `Header{Authorization="Bearer c455a0d8aebc23875b13f1d9ccbd62bf"}` 

If everything works IoT Server, where devices are currently enrolled, will send a `Unautherized` response code with _invalid access token_ payload.

## Scope
This hands-on exercise is meant to give a solid introduction to the nRF5 SDK, as well as the nRF Connect mobile application. In addition, the exercise will show how you can integrate a Bluetooth SIG service into the ble_peripheral_uart application.

## Requirements
This example code has been tested with the nRF52832 DK.
- one nRF52832 DK
- PC running either Windows, MacOS or Linux
- 1x smartphone (Android or iOS)
- 1x micro USB cable
- [Segger Embedded Studio](https://www.segger.com/products/development-tools/embedded-studio/)
- latest version of the [J-Link Software & Documentation Pack](https://www.segger.com/downloads/jlink#)
- [nRF5_SDK_v15.2.0](https://www.nordicsemi.com/Software-and-Tools/Software/nRF5-SDK/Download#infotabs)
- nRF Connect mobile application (download from App Store or Google Play)


## Getting Started
- Download the nRF5 SDK & place it somewhere close to the root C drive (e.g. C:\Nordic\SDK)
- Download Segger Embedded Studio & get the certification setup (see [this video](https://www.youtube.com/watch?v=YZouRE_Ol8g&list=PLx_tBuQ_KSqGHmzdEL2GWEOeix-S5rgTV) for help).
- Download the ble_app_uart_custom_service project from Github & extract it to this location (i.e. nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral)
- Open the SES project (i.e. nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral\ble_app_uart_custom\pca10040\s132\ses\ble_app_uart_pca10040_s132.emProject).
- Open the main.c file & go down to the main() function.
- Take a look at the services_init() function & see how the Nordic UART Service (NUS) is initialized. This service will let us pass data from a cellphone to the nRF52 DK or vice versa.
- Next, take a look at the nus_data_handler function. When we write to the Nordic UART RX characteristic located in the Nordic UART Service, a BLE_NUS_EVT_RX_DATA event is generated. This event is handled by the nus_data_handler function().



- Open the BLE Peripheral HRS (Heart Rate Service) example located here (nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral\ble_app_hrs\pca10040\s132\ses\ble_app_hrs_pca10040_s132.emProject)
- The HRS example contains the 

## Documentation

Refer to the following guides for understanding basic concepts of Bluetooth mesh and architecture of
the Nordic nRF5 SDK for Mesh:
  - @subpage md_doc_getting_started_mesh_quick_start
  - @subpage md_doc_introduction_basic_concepts
  - @subpage md_doc_introduction_basic_architecture
  
## Disclaimer
- The examples are not extensively tested, are only meant for tutorial purposes & should hopefully facilitate a better understanding of the Nordic Mesh SDK.
- NO WARRANTY of ANY KIND is provided.

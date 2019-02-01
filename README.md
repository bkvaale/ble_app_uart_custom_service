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
- Download nRF Connect for Mobile from the Google Play Store or the Apple App Store.
- Download the ble_app_uart_custom_service project from Github & extract it to this location (i.e. nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral)
- Open the SES project (i.e. nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral\ble_app_uart_custom\pca10040\s132\ses\ble_app_uart_pca10040_s132.emProject).
- Open the main.c file & go down to the main() function.
- Take a look at the services_init() function & see how the Nordic UART Service (NUS) is initialized. This service will let us pass data from a cellphone to the nRF52 DK or vice versa.
- Next, take a look at the nus_data_handler function. When we write to the Nordic UART RX characteristic located in the Nordic UART Service, a BLE_NUS_EVT_RX_DATA event is generated. This event is handled by the nus_data_handler function().
- This means that we can easily update the nus_data_handler function to turn on/off a LED when we write a specific string command on our cellphones.
- First off, add this code below the uint32_t err_code line:

        char uart_str[p_evt->params.rx_data.length+1]; // added by BK, used for storing password to turn on light

        for(uint32_t i=0; i<p_evt->params.rx_data.length+1;i++){
            uart_str[i] = 'a';
        }
        
- Then, under the app_uart_put() command, add this line:
uart_str[i] = (char)p_evt->params.rx_data.p_data[i];

- Below the last if block, add this code:

        uart_str[p_evt->params.rx_data.length] = '\0';
        if(strcmp(uart_str,"ledon")==0){
          bsp_board_led_on(LEDBUTTON_LED);
        }
        else if(strcmp(uart_str,"ledoff")==0){
          bsp_board_led_off(LEDBUTTON_LED);
        }

- Now, compile the example with F7, connect to your nRF52 DK by pressing Target -> Connect J-Link -> Erase All. Then, flash the softdevice & application by pressing Target -> Download ble_app_uart_pca10040_s132.

- You should now see the LED1 on the nRF52 DK blinking, which means it is advertising!

- Rename the DEVICE_NAME in main.c to something short & memorable. Open nRF Connect for Desktop & look for the device name you gave. Connect to the device & click the Nordic UART Service. Click the Up Arrow on the Nordic UART RX & write ledon to the characteristic.

- If everything was done correctly, LED 3 should turn on. If you write ledoff to the same characteristic, LED3 should turn off. Next, we'll show how you can send data the other way, from the nRF52 DK to the smartphone via adding a new service, the battery service (BAS).

- Open the BLE Peripheral HRS (Heart Rate Service) example located here (nRF5_SDK_15.2.0_9412b96\examples\ble_peripheral\ble_app_hrs\pca10040\s132\ses\ble_app_hrs_pca10040_s132.emProject)

- The HRS example contains the heart rate service, but also includes the BAS We will take a look at how the BAS is initialized & added in this example & then copy the important code snippets into our own example. 

- Start by taking a look at how the BAS is initialized in the services_init() of the HRS example. Copy these code snippets into your own example:

ble_bas_init_t     bas_init; 

Add this code at the top. Add the next code snippet below the APP_ERROR_CHECK of the Queued Write Module:

    // Initialize Battery Service (BAS), added by BK
    memset(&bas_init, 0, sizeof(bas_init));

    bas_init.evt_handler          = NULL;
    bas_init.support_notification = true;
    bas_init.p_report_ref         = NULL;
    bas_init.initial_batt_level   = 100;

    // Here the sec level for the Battery Service can be changed/increased.
    bas_init.bl_rd_sec        = SEC_OPEN;
    bas_init.bl_cccd_wr_sec   = SEC_OPEN;
    bas_init.bl_report_rd_sec = SEC_OPEN;

    err_code = ble_bas_init(&m_bas, &bas_init);
    APP_ERROR_CHECK(err_code);

- You will also need to add the ble_bas.c file, as well as include the ble_bas.h file at the top of main.c. Under the nRF_BLE_Services folder in SES, right click on the folder -> Add Existing File... Then, navigate to the location of the ble_bas.c file. You can find the file location by right-clicking on the ble_bas.c file in the HRS example & pressing select in File Explorer.

- Once that is done, you can compile & flash the example to the nRF52 DK. Find the DK via nRF Connect again & this time, you should see a battery service option available too. Click on this service. Click on the icon with the numerous arrows & you should see that the battery value is changing. You have successfully sent data from the DK to your phone!

## Documentation

Refer to the following guides for understanding basic concepts of Bluetooth mesh and architecture of
the Nordic nRF5 SDK for Mesh:
  - @subpage md_doc_getting_started_mesh_quick_start
  - @subpage md_doc_introduction_basic_concepts
  - @subpage md_doc_introduction_basic_architecture
  
## Disclaimer
- The examples are not extensively tested, are only meant for tutorial purposes & should hopefully facilitate a better understanding of the Nordic Mesh SDK.
- NO WARRANTY of ANY KIND is provided.

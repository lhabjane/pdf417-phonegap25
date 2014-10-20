pdf417-phonegap25
=================

Backport of Pdf417 Plugin to Phonegap 2.5.0


Android
=================

Copy from AndroidPlugin folder:

- contents of AndroidPlugin/res to your Android project res folder
- contents of AndroidPlugin/libs to your Android project libs folder
- folder AndroidPlugin/com/* to your Android project src folder
- pdf417scanner.js to your Android project assets/www folder

Add permissions and activities to your AndroidManifest.xml:

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.FLASHLIGHT" />        
    
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
    <uses-feature android:name="android.hardware.camera.flash" />
    <uses-feature android:glEsVersion="0x00020000" android:required="false" />

    ....

    <activity
        android:name="mobi.pdf417.activity.Pdf417ScanActivity"
        android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
        android:screenOrientation="portrait" >
        <intent-filter>
            <action android:name="mobi.pdf417.activity.Pdf417ScanActivity" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
	</activity>

In your res/xml/config.xml file add the following line:

    <plugin name="Pdf417Scanner" value="com.phonegap.plugins.pdf417.Pdf417Scanner"/>

Include a reference to plugin JavaScript file:
    
    <script type="text/javascript" src="pdf417scanner.js"></script>

Define options and license keys before starting a scan:
        
        **
        * Scan these barcode types
        * Available: "PDF417", "QR Code", "Code 128", "Code 39", "EAN 13", "EAN 8", "ITF", "UPCA", "UPCE"
        **/
        var types = ["PDF417", "QR Code"];

        /**
        * Initiate scan with options
        * NOTE: Some features are unavailable without a license
        * Obtain your key at http://pdf417.mobi
        **/
        var options = {
            beep : true,
            noDialog : true,
            removeOverlay :true,
            uncertain : false, //Recommended
            quietZone : false, //Recommended
            highRes : false, //Recommended
            frontFace : false
        };

        // Note that each platform requires its own license key

        // This license key allows setting overlay views for this application ID: net.photopay.barcode.pdf417-sample
        var licenseiOs = "YHCP-25S4-ZFR3-3LQR-ANPY-MZFZ-VVHO-YPUW";

        // This license is only valid for package name "mobi.pdf417"
        var licenseAndroid = "FFMV-RULX-VNIU-CSJW-DAC6-JRBU-AKOC-ZV3G";       

Open scan activity like this:

        window.plugins.pdf417Scanner.scanWithOptions(
            // Register the callback handler
            function callback(data) {
                //alert("got result " + data.data + " type " + data.type);
                if (data.cancelled == true) {
                    resultDiv.innerHTML = "Cancelled!";
                } else {
                    resultDiv.innerHTML = "Data: " + data.data + " (raw: " + hex2a(data.raw) + ") (Type: " + data.type + ")";
                }
            },
            // Register the errorHandler
            function errorHandler(err) {
                alert('Error');
            },
            types, options, licenseiOs, licenseAndroid
        );

See more detailed documentation at https://github.com/PDF417/pdf417-phonegap/tree/dev#usage
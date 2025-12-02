# ITL-ALPR-SDK Android Integration Guide

## Integration Steps

1. **Add SDK Dependency**
   - Place the `itlalpr-sdk.aar` file into your project's `app/libs/` directory.
   - Add the following dependency in your `app/build.gradle.kts` file:
     ```kotlin
     implementation(files("libs/itlalpr-sdk.aar"))
     ```

2. **Register and Launch License Plate Recognition Activity**
   - In the `onCreate` method of `MainActivity`, register the launcher for `CameraActivity`:
     ```kotlin
     cameraResultLauncher = registerForActivityResult(
         ActivityResultContracts.StartActivityForResult()
     ) { result ->
         if (result.resultCode == RESULT_OK) {
             // Process recognition results
         }
     }
     ```
   - Launch the license plate recognition activity via a button or other UI element:
     ```kotlin
     val intent = Intent(this, CameraActivity::class.java)
     cameraResultLauncher.launch(intent)
     ```

3. **Obtain Recognition Results**
   - After recognition, get the results from `ScanResultCache`:
     ```kotlin
     val state = ScanResultCache.recognizedState
     val no = ScanResultCache.recognizedNo
     if (!state.isNullOrEmpty() || !no.isNullOrEmpty()) {
         // Recognition succeeded, handle results
     } else {
         // Recognition failed or not registered
     }
     // Clear cache
     ScanResultCache.recognizedState = null
     ScanResultCache.recognizedNo = null
     ```

4. **Error Handling and User Notification**
   - Notify users of recognition results using dialogs or Toasts.
   - Provide friendly messages if recognition fails.

## Contact Information

- Official Website: [https://alpr-sdk.itlogica.net/](https://alpr-sdk.itlogica.net/)
- Email: alpr-sdk@itlogica.com

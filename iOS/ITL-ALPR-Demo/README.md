# OCRSDK Usage Guide

This guide details how to integrate and use OCRSDK for license plate recognition in iOS projects.

## 1. SDK Integration

### 1.1 Add SDK to Project
Add the `OCRSDK.xcframework` folder to the root directory of your Xcode project, and include it in Frameworks, Libraries, and Embedded Content in your project settings.

### 1.2 Import Header File
Import the header file in the class where you need to use the SDK:
```objective-c
#import <OCRSDK/OCRSDK.h>
```

## 2. Initialize SDK

Create an OCRSDK instance in your view controller and pass in the preview view controller (usually self):
```objective-c
self.sdk = [[OCRSDK alloc] initWithPreview:self];
```

## 3. Start Recognition

Call the `startRecognize` method to start license plate recognition:
```objective-c
[self.sdk startRecognize:^(NSMutableArray<NSString *> *resultArray, UIImage *processedImage) {
    // Handle recognition results
}];
```

- `resultArray`: Array of recognition results, usually the first element is the state name, and the second is the license plate number.
- `processedImage`: The processed image, which can be displayed or saved.

## 4. Handling Recognition Results

It is recommended to update the UI on the main thread in the callback, such as showing a popup with the recognition results:
```objective-c
dispatch_async(dispatch_get_main_queue(), ^{
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"Recognition Result"
                                                                   message:[NSString stringWithFormat:@"State: %@\nPlate: %@", state, plate]
                                                            preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [self presentViewController:alert animated:YES completion:nil];
});
```

## 5. Resource File Description

The SDK contains multiple models and configuration files. Make sure `OCRSDKResources.bundle` is correctly added to your project.

- License plate recognition models: such as `ch_ppocr_mobile_v2.0_det_slim_opt.nb.enc`, etc.
- Configuration files: `config.txt`, `label.txt.enc`, etc.

## 6. FAQ

- **SDK cannot recognize or crashes**: Please check if the resource files are complete and the paths are correct.
- **Recognition result is empty**: Make sure the image is clear and the license plate is not blocked.
- **Compilation error after integration**: Confirm that the xcframework is correctly added and the header file is imported.

## 7. Sample Code

```objective-c
#import <OCRSDK/OCRSDK.h>

@interface ViewController : UIViewController
@property (nonatomic, strong) OCRSDK *sdk;
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    self.sdk = [[OCRSDK alloc] initWithPreview:self];
}
- (void)recognizeButtonTapped {
    [self.sdk startRecognize:^(NSMutableArray<NSString *> *resultArray, UIImage *processedImage) {
        NSString *state = (resultArray.count > 0) ? resultArray[0] : @"Unrecognized";
        NSString *plate = (resultArray.count > 1) ? resultArray[1] : @"Unrecognized";
        dispatch_async(dispatch_get_main_queue(), ^{
            UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"Recognition Result"
                                                                           message:[NSString stringWithFormat:@"State: %@\nPlate: %@", state, plate]
                                                                    preferredStyle:UIAlertControllerStyleAlert];
            [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
            [self presentViewController:alert animated:YES completion:nil];
        });
    }];
}
@end
```

## 8. Contact Information

- Official Website: [https://alpr-sdk.itlogica.net/](https://alpr-sdk.itlogica.net/)
- Email: alpr-sdk@itlogica.com

# CC-AfterHours-ObjCData
CC's After Hours project on how to parse and display JSON data in Objective-C.

## Step 1: Setup
1. Fork and Clone this repo.
2. Open the `CDBCovid.xcodeproj` starter file.
3. Review the storyboard scene.
4. Click on your (blue) project, open right panel, click on file inspector, and fill out the class prefix with 3 initials. 
(I use my initials - CDB) 
5. Review API results: 
   * For the World: https://covid19.mathdro.id/api/
   * For a country: https://covid19.mathdro.id/api/countries/USA

------
## Step 2: Model, Model Controller, and View Controller Files

### Model:
1. Create a new group called "Model". Then create a new cocoa touch class file, named `CDBCoronaData` and subclassed it from `NSObject`. 
2. In the `CDBCoronaData.h` header file:
    * Create properties for confirmed, deaths, and recovered.
    * Create methods.
3. In the `CDBCoronaData.m` implementation file:
    * Copy/Paste your methods over from your header file.
    * Write your boiler plate code for init and set your instance variables.
     ```
     self = [super init]; // boiler plate code
     if (self) {
         // set instance variables
         _confirmed = confirmed;
         _deaths = deaths;
         _recovered = recovered;
     }
     return self;
     ```
    * Set self with dictionary values. 
    `self = [self initWithConfirmed:[confirmed intValue] deaths:[deaths intValue] recovered:[recovered intValue]];`

### Model Controller:
1. Create a new group called "Model Controller". Then create a new cocoa touch class file, named `CDBCoronaDataController` and subclassed it from `NSObject`.
2. In the `CDBCoronaDataController.h` header file:
    * Set your class forward declaration. `@class CDBCoronaData;`
    * Create a property for the model
    * Create 3 methods: `getWorldData`, `getCountryData`, `initWithDictionary`
3. In the `CDBCoronaDataController.m` implementation file:
    * Import the model `CDBCoronaData.h` into the top of this file. 
    * Copy/Paste methods over from your header file.
    * In the `getWorldData` method:
      * Set up your baseURL, request, and data task. (Handle errors and data, create a model object/alloc the space/initalize it, and don't forget to resume!)
    * In the `getCountryData` method:
      * Set up your baseURL, request, and data task. (Handle errors and data, create a model object/alloc the space/initalize it, and don't forget to resume!)
    * In the `initWithDictionary` method:
      * Create a model object/alloc the space/initalize it, boiler plate code, set instance variable, and return self.
      ```
      CDBCoronaData *coronaData = [[CDBCoronaData alloc] initWithDictionary: dictionary];
      self = [super init]; // Boiler plate code
      if (self) {
          _coronaData = coronaData; // set instance variable
      }
      return self;
      ```
      
### View Controller:
1. Create a new group called "View Controller". Then create a new cocoa touch class file, named `CDBViewController` and subclassed it from `UIViewController`.
2. Set storyboard scene to this class. 
3. In the `CDBViewController.m` implementation file:
  * Import the model `CDBCoronaData.h` and the model controller `CDBCoronaDataController.h` into the top of this file.  
  * Create a property for the model controller. `@property (nonatomic) CDBCoronaDataController *coronaDataController;`
  * Create outlets.
  * Create action for search button.
  * Create method `getWorldData` in interface. `- (void)getWorldData;`
  * In `viewDidLoad`:
    * Allocate the space and initalize the `coronaDataController`
    * Call the `getWorldData` method
  * In `getWorldData`:
    * Call this function from the imported `CDBCoronaDataController`.
  * In `searchButtonTapped` action:
    * When a user clicks the search button, dismiss the keyboard.
    `[[UIApplication sharedApplication] sendAction:@selector(resignFirstResponder) to:nil from:nil forEvent:nil];`
    * If text on the searchTextField == "World", then call this class' function getWorldData
    ```
    if ([self.searchTextField.text isEqualToString:@"World"]) {
        [self getWorldData];
    ```
    * Otherwise set the country name label to the search term text, call the function getCountryData, and set the labels to the country's data for countryName/confirmed/deaths/recovered. 
    ```
    } else {
        self.countryNameLabel.text = self.searchTextField.text;
        [self.coronaDataController getCountryData:self.searchTextField.text completion:^(CDBCoronaData *coronaData) {
            dispatch_async(dispatch_get_main_queue(), ^{
                self.countryNameLabel.text = self.searchTextField.text;
                self.confirmedLabel.text = [NSString stringWithFormat:@"%d", coronaData.confirmed];
                self.deathsLabel.text = [NSString stringWithFormat:@"%d", coronaData.deaths];
                self.recoveredLabel.text = [NSString stringWithFormat:@"%d", coronaData.recovered];
            });
        }];
    }
    ```
    
------
## Step 3: Last Steps
1. Hook up the outlets in the storyboard to the view controller file. 
2. Build and run the project. 
3. You can test different countries like "USA", "Italy", or "China", and see the country's confirmed, death, and recovered cases. 


------
## Solution Code:
Please visit this [repo](https://github.com/ladybeitel/CC-AfterHours-ObjCData-SolutionCode). 

MGCurrencyExchanger
===================

iOS and Mac OS X Currency Exchanger

An online and offlne currency exchanger for Mac OS X and iOS. This is a very simple library and very easy to use.      
Online rates provided by Yahoo!®



To start off, just add the following files to your bundle:
*	MGCountryNames.h
*	MGCurrencyExchanger.h
*	MGCurrencyExchanger.m
*	NSLocale+CurrencyExtension.h
*	NSLocale+CurrencyExtension.m

Optionally add this file:
* ExchangeRates.csv

Then `#import MGCurrencyExchanger.h` into your class to start exchanging rates.

`MGCurrencyExchanger` uses `NSLocale` for the currencies. It is very simple and has just two methods along with some convenience methods. The two methods are:   
`+(double)exchangeAmountOnline:(double)amount fromCurrency:(NSLocale *)fromLocale toCurrency:(NSLocale *)toLocale withError:(NSError **)error;`   
and    
`+(double)exchangeAmountOffline:(double)amount fromCurrency:(NSLocale *)fromLocale toCurrency:(NSLocale *)toLocale fromFile:(NSArray *)csvFile withError:(NSError **)error;`      
      
The first being more accurate and up to date while the other can be as accurate depending on when you generate the `ExchangeRates.csv`.

Online Exchange
================

Then follow these steps to start exchanging online:
    
    //Set up locales. Locales are required for this library to work
    NSLocale *unitedStatesLocale = [NSLocale localeFromCountryName:MGCountryUnitedStates];
    NSLocale *unitedKingdomLocale = [NSLocale localeFromCountryName:MGCountryUnitedKingdom];
    
    NSError *error;
    
    double amountToExchange = 10.00;
    
    double exchangedAmount = [MGCurrencyExchanger exchangeAmountOnline:amountToExchange fromCurrency:unitedStatesLocale toCurrency:unitedKingdomLocale withError:&error];
    
    if (!error)
        NSLog(@"%f", exchangedAmount);  //Prints around 5.8xxx
    
    //That's it! Thats how easy it is.

Offline Exchange
=================

For offline conversion it's a tad bit more complicated. First, you'll need to generate an `ExchangeRates.csv` (Note the file is not required to be titled that). In order to do this, check out my gist I created which will do this for you. The link is here: [Exchange Rate Generator](https://gist.github.com/GenralDesigns/62129d0ed577f69df19b "Exchange Rate Generator for iOS and Mac OS X"). It's very simple just copy and paste the method into your view controller or whatever. Then run your program and it'll finish within about five minutes. If you don't want to generate one yourself, you can download the one I included which will be updated every month. The final file is about 400KB, very small so it can be loaded into memory without worry. Then add the file to your bundle.
    Then do the following:
    
    NSString *csvFileString = [NSString stringWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"ExchangeRates" ofType:@"csv"] encoding:NSUTF8StringEncoding error:nil];
        
    NSArray *csvFile = [csvFileString componentsSeparatedByString:@","];
        
    NSLocale *unitedStatesLocale = [NSLocale localeFromCountryName:MGCountryUnitedStates];
    NSLocale *unitedKingdomLocale = [NSLocale localeFromCountryName:MGCountryUnitedKingdom];
    
    NSError *error;
    
    double amountToExchange = 10.00;
    
    double exchangedAmount = [MGCurrencyExchanger exchangeAmountOffline:amountToExchange fromCurrency:unitedStatesLocale toCurrency:unitedKingdomLocale fromFile:csvFile withError:&error];
    
    if (!error)
        NSLog(@"%f", exchangedAmount);  //Again prints around 5.8xxx
    
    //That's it! Thats how easy it is for offline conversion!
    
    
Helper methods
==============
    
I've also added a few helpful methods to aid you on your exchanging adventures, the first being `MGCountryNames`. This gives all the locale identifiers for every country available in iOS. Use it like so:      
`NSLocale *locale = [NSLocale localeWithCountryName:MGCountryAustralia`
    
The next method is `-(NSString *)currencyCode;` this method will return the currency code that corresponds to whatever locale you give it. Likewise, `-(NSString *)currencySymbol;` returns the symbol. 

`-(NSString *)currencyNameFromCurrencyCode` will return the name for the currency you give it in whatever language the locale you give it is.


`+(NSLocale *)localeFromCurrencyCode:(NSString *)currencyCode;` creates a locale from a given currency code. All of the above methods are extensions of `NSLocale`.


Finally, there's one more method: `+(NSString *)formattedCurrencyWithAmount:(double)amount currency:(NSLocale *)currencyLocale symbol:(BOOL)symbolBool;`. This does exactly like it says. It takes an amount, a currency, and a `BOOL` saying whether or not to format in the currency symbol. This localizes the comma and decimal point separators. 

    
    





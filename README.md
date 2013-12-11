# Objective-Shorthand

Objective-Shorthand is a set of categories that make long things in Objective-C short. Additions are encouraged.

## What can Objective-Shorthand help with?

### Regular Expressions

Objective-Shorthand shortens the code for seeing if a regular expression matches a string from:

    NSRegularExpression *regularExpression = [NSRegularExpression regularExpressionWithPattern:regex options:0 error:nil];
    NSUInteger numberOfMatches = [regularExpression numberOfMatchesInString:self options:0 range:NSMakeRange(0, self.length)];
	BOOL doesMatch = numberOfMatches > 0;

to:

    BOOL doesMatch = [string matchesRegex:regex];

A method to find the range of the first match for a given regex is also included:

	- (NSRange) rangeOfFirstSubstringMatching:(NSString*)regex;

### JSON

Using `NSJSONSerialization`, you have convert your strings to `NSData` before deserializing them to arrays and dictionaries. Objective-Shorthand simplifies all that.

It provides the same interface as `JSONKit`, but it works with the build Apple JSON serializer behind the scenes. The Ruby community is very good at writing libraries with consistent interfaces, and this is something we need to steal as writers of Objective-C. 

To convert from a JSON string to an `NSArray` or `NSDictionary`:

	- (id) objectFromJSONString;

And to convert from an array or dictionary to an `NSString`:

	- (NSString *)JSONString;

### NSComparisonMethods

The OS X SDK has a weird API called [NSComparisonMethods](https://developer.apple.com/library/mac/documentation/cocoa/reference/foundation/Protocols/NSComparisonMethods_Protocol/Reference/Reference.html). Basically, this API provides a wrapper around `compare:` and uses it define the following methods:

	- (BOOL)isEqualTo:(id)object;
	- (BOOL)isLessThanOrEqualTo:(id)object;
	- (BOOL)isLessThan:(id)object;
	- (BOOL)isGreaterThanOrEqualTo:(id)object;
	- (BOOL)isGreaterThan:(id)object;
	- (BOOL)isNotEqualTo:(id)object;

For some reason, this never made it over to iOS, even though it's tremendously useful. I like these methods because they're way more semantic than `[object compare:otherObject] == NSOrderedDescending` is way harder to understand than `[object isGreaterThan:otherObject]`. 

If the object in question doesn't respond to `compare:` an exception will be thrown.

### Array Uniquing

Pulling out the unique elements of an array involves the ever-goofy `[array valueForKeyPath:@"@distinctUnionOfObjects.self"]`. This is wrapped up inside of the following method:

	- (NSArray*) arrayByUniquingObjects;

You never have to remember how to type that string literal again! Autocomplete Rules Everything Around Me.

### Functional Collection Operators

Finally, Objective-Shorthand provides a category on `NSArray` that provides the normal functional collection operators, like `map`, `select`, and `any`, but with names that are both more semantic and more native to Objective-C.

`each` is already defined in Foundation, so it is not included.

	- (void)enumerateObjectsUsingBlock:(void (^)(id obj, NSUInteger idx, BOOL *stop))block;

The other operators can be found, including `select` (aka `filter`) and `reject`:

	- (NSArray*) arrayBySelectingItemsPassingTest:(BOOL (^)(id object))test;
	- (NSArray*) arrayByRejectingItemsPassingTest:(BOOL (^)(id object))test;

`map` (aka `collect`) and `reduce` (aka `inject`) are available as well.

	- (NSArray*) arrayByTransformingObjectsUsingBlock:(id (^)(id object))block;
	- (id) objectByReducingObjectsIntoAccumulator:(id)accumulator usingBlock:(id (^)(id accumulator, id object))block;

And finally, some boolean operators:

	- (BOOL) allObjectsPassTest:(BOOL (^)(id object))test;
	- (BOOL) anyObjectsPassTest:(BOOL (^)(id object))test;
	- (BOOL) noObjectsPassTest:(BOOL (^)(id object))test;

## Tests

Pull requests are welcome, however, there is a test suite. Please make sure that you are not breaking the tests, and if you add new methods, make sure there is test coverage for it.
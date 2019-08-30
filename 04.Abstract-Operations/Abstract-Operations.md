# 7 Abstract Operations

## 7.1 Type Conversion

### 7.1.1 ToPrimitive ( input [ , PreferredType ] )

The abstract operation ToPrimitive takes an input argument and an optional argument PreferredType. The abstract operation ToPrimitive converts its input argument to a non-Object type. If an object is capable of converting to more than one primitive type, it may use the optional hint PreferredType to favour that type. Conversion occurs according to the following algorithm:

1. Assert: input is an ECMAScript language value.
2. If Type(input) is Object, then
   1. If PreferredType is not present, let hint be "default".
   2. Else if PreferredType is hint String, let hint be "string".
   3. Else,
      1. Assert: PreferredType is hint Number.
      2. Let hint be "number".
   4. Let exoticToPrim be ? `GetMethod(input, @@toPrimitive)`.
   5. If exoticToPrim is not undefined, then
      1. Let result be ? `Call(exoticToPrim, input, « hint »)`.
      2. If `Type(result)` is not Object, return result.
      3. Throw a TypeError exception.
   6. If hint is "default", set hint to "number".
   7. Return ? `OrdinaryToPrimitive(input, hint)`.
3. Return input.

#### 7.1.1.1 OrdinaryToPrimitive ( O, hint )

When the abstract operation OrdinaryToPrimitive is called with arguments O and hint, the following steps are taken:

1. Assert: Type(O) is Object.
2. Assert: Type(hint) is String and its value is either "string" or "number".
3. If hint is "string", then
   Let methodNames be « "toString", "valueOf" ».
4. Else,
   Let methodNames be « "valueOf", "toString" ».
5. For each name in methodNames in List order, do
   1. Let method be ? `Get(O, name)`.
   2. If `IsCallable(method)` is true, then
      1. Let result be ? `Call(method, O)`.
      2. If `Type(result)` is not Object, return result.
6. Throw a TypeError exception.

### 7.1.2 ToBoolean(argument)

The abstract operation ToBoolean converts argument to a value of type Boolean according to Table 9:

![toboolean]('../images/7_1_2_toBoolean.png')

### 7.1.3 ToNumber(argument)

The abstract operation ToNumber converts argument to a value of type Number according to Table 10:
![toNumber]('../images/7_1_3_number.png')

#### 7.1.3.1 ToNumber Applied to the String Type

ToNumber applied to Strings applies the following grammar to the input String interpreted as a sequence of UTF-16 encoded code points (6.1.4). If the grammar cannot interpret the String as an expansion of StringNumericLiteral, then the result of ToNumber is NaN.

> The terminal symbols of this grammar are all composed of characters in the Unicode Basic Multilingual Plane (BMP). Therefore, the result of ToNumber will be NaN if the string contains any leading surrogate or trailing surrogate code units, whether paired or unpaired.

![syntax]('../images/7_1_3_1.png)

## 7.2 Testing and Comparison Operations

## 7.3 Operations on Objects

## 7.4 Operations on Iterator Objects

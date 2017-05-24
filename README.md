[TOC]

# Task documentation #

## Csv.Parse ##

### Input ###

| Property             | Type                 | Description                          |
| ---------------------| ---------------------| ------------------------------------ |
| Csv                  | string               | Input csv as a string                                                                    |          
| Delimiter            | string               | The separator used in the csv string                                                     |
| ColumnSpecifications | ColumnSpecification[]| Used for mapping csv columns into types. The order of the columns are used for mapping.  |

#### ColumnSpecification
| Property             | Type                 | Description                          |
| ---------------------| ---------------------| ------------------------------------ |
| Name                 | string               | Name of the column. Does not have to match the one in the csv string. The order of the ColumnSpecification elements are matched ot the order of columns in the csv string  |
| Type                 | Enum{String, Int, Long, Decimal, Double, Boolean, DateTime, Char} | Type of the resulting column |

### Options ###

| Property             | Type                 | Description                          |
| ---------------------| ---------------------| ------------------------------------ |
| ContainsHeaderRow    | bool                 | This flag tells the reader if there is a header row in the CSV string. Default is true. |  
| SkipRowsFromTop      | int                  | This value is set to ignore a specific amount of rows from the beginning of the csv string. This can for example be used if the string contains some metadata on the first row before the header.  |  
| SkipEmptyRows        | bool                 | A flag to let the reader know if a record should be skipped when reading if it's empty. A record is considered empty if all fields are empty.                |  
| TrimOutput           | bool                 | This flag tells the reader to trim whitespace from the beginning and ending of the field value when reading.              |  
| CultureInfo          | string               | The culture info to parse the file with, e.g. for decimal separators. InvariantCulture will be used by default. See list of cultures [here](https://msdn.microsoft.com/en-us/library/ee825488(v=cs.20).aspx); use the Language Culture Name. <br> NOTE: Due to an issue with the CsvHelpers library, all CSV tasks will use the culture info setting of the first CSV task in the process; you cannot use different cultures for reading and parsing CSV files in the same process.|  

### Example usage ###

![CsvParseUsage.png](https://bitbucket.org/repo/xKXeyR/images/1591752149-CsvParseUsage.png)

### Result ###
Object ParseResult

| Property/Method      | Type                 | Description                          | Example |
| ---------------------| ---------------------| ------------------------------------ |---------| 
| Data                 | List<List<object>>   | Result Data contains each data row from the input csv | `[["Example", 5, true], ["Other", 4, false]]`|  
| Headers              | List<string>         | Result Headers contain the header row of the input csv/ColumnSpecifications | `["Name", "Id", "IsMember"]`|
| ToJson()             | JToken               | Converts the Data and Headers to a Json array   | `[{"Name": "Example", "Id": 5, "IsMember": true}, {"Name": "Other", "Id": 4, "IsMember": false}]` |
| ToXml()              | string               | Converts the Data and Header to a xml string    | `<Root> <Row><Name>Example</Name><Id>5</Id><IsMember>true</IsMember></Row> <Row><Name>Other</Name><Id>4</Id><IsMember>false</IsMember></Row> </Root>`


## Csv.Create ##

### Input ###

| Property             | Type                 | Description                          | Example   |
| ---------------------| ---------------------| ------------------------------------ |-----------|
| InputType            | Enum { List, Json }  | Input csv as a string                |           |
| Json                 | string               | If InputType is Json this field will be visible and used. Input needs to be an Array of Objects | `[ {"Header1": "Value1", "Header2": "Value2"}, {"Header1": "Other1", "Header2": "Other2"} ]` 
| Headers              | List<string>         | If inputType is List this field will be visible and used. Headers for the Data. Need to be in the same order as the underlying Data | `new List<string> {"Header1", "Header2"}` |
| Data                 | List<List<object>>   | If inputType is List this field will be visible and used. The order of the nested list objects need to be in the same order as the header list. | `new List<List<object>>() { new List<object> {"Value1", "Value2"}, new List<object> {"Other1", "Other2"}`|
| Delimiter            | string               | The separator used in the csv string | `;`     

### Options ###

| Property             | Type                 | Description                          |
| ---------------------| ---------------------| ------------------------------------ |
| IncludeHeaderRow     | bool                 | This flag tells the writer if a header row should be written. |  
| CultureInfo          | string               | The culture info to write the file with, e.g. for decimal separators. InvariantCulture will be used by default. See list of cultures [here](https://msdn.microsoft.com/en-us/library/ee825488(v=cs.20).aspx); use the Language Culture Name. <br> NOTE: Due to an issue with the CsvHelpers library, all CSV tasks will use the culture info setting of the first CSV task in the process; you cannot use different cultures for reading and parsing CSV files in the same process. |  

### Example usage ###

### Result ###
Object CreateResult

| Property/Method      | Type                 | Description                          |
| ---------------------| ---------------------| ------------------------------------ |
| Csv                  | string               | Resulting csv string                 |
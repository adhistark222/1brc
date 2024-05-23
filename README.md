# 1brc
My understanding and implementation of the original 1 billion challenge. [https://github.com/gunnarmorling/1brc]

## Problem Statement:

**Modified from [the original repository](https://github.com/gunnarmorling/)**

The text file (measurements.txt) contains temperature values for a range of weather stations.
Each row is one measurement in the format `<string: station name>;<double: measurement>`, with the measurement value having exactly one fractional digit.
The following shows ten rows as an example:

```
Hamburg;12.0
Bulawayo;8.9
Palembang;38.8
St. John's;15.2
Cracow;12.6
Bridgetown;26.9
Istanbul;6.2
Roseau;34.4
Conakry;31.2
Istanbul;23.0
```

The task is to write a python program which reads the file, calculates the min, mean, and max temperature value per weather station, and emits the results on stdout like this
(i.e. sorted alphabetically by station name, and the result values per station in the format `<min>/<mean>/<max>`, rounded to one fractional digit):

```
{Abha=-23.0/18.0/59.2, Abidjan=-16.2/26.0/67.3, Abéché=-10.0/29.4/69.0, Accra=-10.1/26.4/66.4, Addis Ababa=-23.7/16.0/67.0, Adelaide=-27.8/17.3/58.5, ...}
```

## Initial thoughts and understanding of the problem
- I need to create a new text file measurements.txt from the given example data in the csv file.
- The original repository has a python script under `1brc/src/main/python/create_measurements.py` [here](https://github.com/gunnarmorling/1brc/blob/main/src/main/python/create_measurements.py) to create a measurements.txt with 1 Billion rows so I don't have to write it.
- I need to write a script for the expected result which is supposed to be **sorted** alphabetically by station name with results printed as `<min>/<mean>/<max>`, rounded to one fractional digit. Eg: `Abha=-23.0/18.0/59.2`

### What could be done
1. When we have a dataset this large (the text file generated with billion rows is said to be more than 13 GB in the original repository) 
- The brute force solution would be 
    - To read the text file into a pandas DataFrame.
    - Group by station and calculate min, mean, and max
    - Convert the result to a dictionary for the desired output format and print the results.
    - However, as Python is not low-level and performance-focused as languages like Java,C or C++. So for decent performance requirements in such extensive dataset, I need to consider other optimization techniques.
2. I could try to chunk up the large data just like we would in a merge sort or a quick sort and try solving the chunked up bits first and and solve by merging them recursively. 
    - I haven't been exposed to parallel prosessing yet but if I could try to process 8 chunks parallely in my 8-core computer it would drastically improve performance. 
    #### Input and output formats
``` console
**Inputs**:
  description: "The input file format for the modified code."
  structure:
    - file_path: "String"
      description: "The path to the input text file containing temperature measurements."
      example: "measurements.txt"
    - max_cpu: "Integer (optional)"
      description: "The maximum number of CPU cores to utilize for parallel processing."
      example: 8

**Outputs**:
  description: "The output format produced by the modified code."
  structure:
    - location: "String"
      description: "The name of the location or weather station."
      example: "New York"
    - temperature_statistics:
      description: "Temperature statistics for the location."
      structure:
        - min_temperature: "Float"
          description: "The minimum temperature recorded at the location."
          example: 10.5
        - mean_temperature: "Float"
          description: "The mean temperature calculated from the sum of temperatures divided by the count of measurements."
          example: 15.2
        - max_temperature: "Float"
          description: "The maximum temperature recorded at the location."
          example: 20.8
```
`Solution is outlined in calculate_output.py yet to be uploaded` [here]()


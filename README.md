# Databases Hackerrank
![choosing-right-database-for-your-applications](https://user-images.githubusercontent.com/46414243/132398024-5e6ffec0-ab7e-46b1-9e30-803dded4003e.png)

## Basics of Sets and Relations #1
```
8
```

## Basics of Sets and Relations #2
```
5
```
## Basics of Sets and Relations #3
```
1
```
## Basics of Sets and Relations #4
```
42
```
## Basics of Sets and Relations #5
```
2
```
## Basics of Sets and Relations #6
```
2
```
## Basics of Sets and Relations #7
```
2
```
## Relational Algebra - 3
```
Equijoins
```
## Relational Algebra - 4
```
Left to right
```
## Database Query Languages
```
Query
```
## Procedural Language
```
Relational algebra
```
## Relations - 1
```
Join
```
## Relations - 2
```
Cartesian product
```
## Index Architecture Types
```
2
```
## Indexes - 2
```
If the table has a clustered index, or the index is on an indexed view, the row locator is the clustered index key for the row.
```
## Indexes - 3
```
A = 1.33B
```
## OLAP Performance
```
Aggregation
```
## OLAP Operations - 1
```
roll-up
```
## OLAP Operations - 2
```
pivot
```
## OLAP Cube Metadata
```
Both star and snowflake schema(s)
```
## OLAP Name(s)
```
Both (B) and (C)
```
## The Total View
```
Data Warehousing
```
## OLAP Operation Types
```
(4, 7, 3, 84, 160, 117)
```
## Databases - Relational Calculus
```
19
```
## Databases - Keys
```
bookname
```
## Databases - Natural Joins
```
50
1
6
```
## Databases - Differences
```
2
3
```
## Map Reduce Advanced - Count number of friends
```
import sys
from collections import OrderedDict
class MapReduce:
    def __init__(self):
        self.intermediate = OrderedDict()
        self.result = []
   

    def emitIntermediate(self, key, value):
   	self.intermediate.setdefault(key, [])       
        self.intermediate[key].append(value)

    def emit(self, value):
        self.result.append(value) 

    def execute(self, data, mapper, reducer):
        for record in data:
            mapper(record)

        for key in self.intermediate:
            reducer(key, self.intermediate[key])

        self.result.sort()
        for item in self.result:
            print "{\"key\":\""+item[0]+"\",\"value\":\"" + str(item[1]) + "\"}"

mapReducer = MapReduce()

def mapper(record):
    #Start writing the Map code here
    ls = record.split()
    mapReducer.emitIntermediate(ls[0],ls[1])
    mapReducer.emitIntermediate(ls[1],ls[0])

def reducer(key, list_of_values):
    #Start writing the Reduce code here
    mapReducer.emit((key,len(list_of_values)))

if __name__ == '__main__':
  inputData = []
  for line in sys.stdin:
   inputData.append(line)
  mapReducer.execute(inputData, mapper, reducer)
```
## Map Reduce Advanced - Relational Join
```
import sys
from collections import OrderedDict
class MapReduce:
    def __init__(self):
        self.intermediate = OrderedDict()
        self.result = []
   

    def emitIntermediate(self, key, value):
   	self.intermediate.setdefault(key, [])       
        self.intermediate[key].append(value)

    def emit(self, value):
        self.result.append(value) 

    def execute(self, data, mapper, reducer):
        for record in data:
            mapper(record)

        for key in self.intermediate:
            reducer(key, self.intermediate[key])

        self.result.sort()
        for item in self.result:
            print item

mapReducer = MapReduce()

def mapper(record):
    #Start writing the Map code here
    fields = record.replace('\n', '').split(',')
    
    if fields[0] == 'Employee':
        person_name = fields[1]
        ssn = fields[2]
        mapReducer.emitIntermediate(ssn, (fields[0], person_name))
    else:
        ssn = fields[1]
        department_name = fields[2]
        mapReducer.emitIntermediate(ssn, (fields[0], department_name))

def reducer(key, list_of_values):
    #Start writing the Reduce code here
    person_names = []
    department_names = []
    for label, value in list_of_values:
        if label == 'Employee':
            person_names.append(value)
        else:
            department_names.append(value)
    
    for person_name in person_names:
        for department_name in department_names:
            mapReducer.emit((key, person_name, department_name))
if __name__ == '__main__':
  inputData = []
  for line in sys.stdin:
   inputData.append(line)
  mapReducer.execute(inputData, mapper, reducer)
```
## Map Reduce Advanced - Matrix Multiplication
```
import sys
from collections import OrderedDict
import itertools

class MapReduce:
    def __init__(self):
        self.intermediate = OrderedDict()
        self.result = []

    def emitIntermediate(self, key, value):
        self.intermediate.setdefault(key, [])
        self.intermediate[key].append(value)


    def emit(self, value):
        self.result[value[0]][value[1]] = value[2]


    def execute(self, matrix1, matrix2, mapper, reducer):
        n = len(matrix1)
        m = len(matrix2[0])
        for i in xrange(0, n):
            self.result.append([0] * m)

        mapper(matrix1, matrix2)

        for key in self.intermediate:
            reducer(key, self.intermediate[key])

        for i in xrange(0, n):
            row = ""
            for j in xrange(0, m):
                row += str(self.result[i][j]) + " "
            print(row)

mapReducer = None

def mapper(matrix1, matrix2):
    #Start writing the Map code here
    n = len(matrix1)
    k = len(matrix1[0])
    m = len(matrix2[0])
    
    for i in xrange(n):
        for j in xrange(m):
            for p in xrange(k):
                mapReducer.emitIntermediate((i, j), (matrix1[i][p], matrix2[p][j]))

def reducer(key, list_of_values):
    #Start writing the Reduce code here
    mapReducer.emit((key[0], key[1], sum(itertools.starmap(lambda x, y: x * y, list_of_values))))
    
if __name__ == '__main__':
    testcases = int(raw_input())
    for _ in xrange(testcases):
        mapReducer = MapReduce()
        dimensions = sys.stdin.readline().strip().split(" ")
        row = int(dimensions[0])
        column = int(dimensions[1])
        matrix1 = []
        matrix2 = []
        
        for i in range(row):
            read_row = sys.stdin.readline().strip()
            matrix1.append([])
            row_elems = read_row.strip().split()
            for j in range(0, len(row_elems)):
                matrix1[i].append(int(row_elems[j]))
        dimensions = sys.stdin.readline().strip().split(" ")
        row = int(dimensions[0])
        column = int(dimensions[1])
        
        for i in range(row):
            read_row = sys.stdin.readline().strip()
            matrix2.append([])
            row_elems = read_row.strip().split()
            for j in range(0, len(row_elems)):
                matrix2[i].append(int(row_elems[j]))
        
        mapReducer.execute(matrix1, matrix2, mapper, reducer)
```
## Querying XML Datastores with XPath - 1
```
collection/movie/@title
```
## Querying XML Datastores with XPath - 2
```
collection/movie/popularity/text()
```
## Querying XML Datastores with XPath - 3
```
collection/movie[popularity/text() <8]/format/text()
```
## Querying XML Datastores with XPath - 4
```
collection/movie[@title ='Trigun']/popularity/text()
```
## Querying XML Datastores with XPath - 5
```
collection/movie[@title='Transformers']/@shelf
```
## Querying XML Datastores with XPath - 6
```
collection/movie[contains(description, 'bit')]/@title
```
## Querying XML Datastores with XPath - 7
```
sum(collection/movie/popularity) div count(collection/movie/popularity)
```
## Querying XML Datastores with XPath - 8
```
string-length(collection/movie[2]/description/text())
```
## Querying XML Datastores with XPath - 9
```
collection/movie[@shelf='B'][2]/@title
```
## Querying XML Datastores with XPath - 10
```
collection/movie[position() > last() - 2]/@title
```
## Querying XML Datastores with XPath - 11
```
collection/movie[position() mod 2 != 0]/@title
```
## Database Normalization #1 - 1NF
```
3
5
2
```
## Database Normalization #2 - 1/2/3 NF
```
3
```
## Database Normalization #3
```
3
```
## Database Normalization #4
```
10
```
## Database Normalization #5
```
3
```
## Database Normalization #6
```
2
```
## Database Normalization #7
```
3.5
```
## Database Normalization #8
```
3
```
## Database Normalization #9
```
2
```
## Database Normalization #10
```
6
```

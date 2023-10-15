# Task 3

## Dataset

* Name: UK Property Price official data 1995-202304

* Size: $4.6\ Gb$
* Link: https://www.kaggle.com/datasets/lorentzyeung/price-paid-data-202304

## Map-Reduce (Sum)

* Mapper:

  ```python
  #!/usr/bin/env python3
  
  import sys
  
  for line in sys.stdin:
      price = int(line.strip().split(',')[1].strip('\'"'))
      print('{}\t{}'.format(1, price))
  ```

* Reducer:

  ```python
  #!/usr/bin/env python3
  import sys
  
  total = 0
  for line in sys.stdin:
      _, price = line.strip().split('\t')
      price = int(price)
      total += price
  print(price)
  ```

## Resource Usage

* No Hadoop: TODO
* Hadoop Standalone
  * Time: 5min, 9sec
  * CPU: 1791 vcore-seconds
  * Memory:  2179437 MB-seconds
* Hadoop Cluster
  * Time: 4mins, 14sec
  * CPU: 1552 vcore-seconds
  * Memory: 1875994 MB-seconds
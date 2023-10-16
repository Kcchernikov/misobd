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

## Resource Usage (Sum operation)

|        | [No Hadoop (1 thread)](#Simple runner) | Hadoop Standalone  | Hadoop Cluster (3 nodes) |
| ------ | -------------------------------------- | ------------------ | ------------------------ |
| Time   | 3min, 10sec                            | 5min, 9sec         | 4min, 14sec              |
| CPU    | 97 vcore-seconds                       | 1791 vcore-seconds | 1552 vcore-seconds       |
| Memory | 19.7 MB/s                              | 2179437 MB-seconds | 1875994 MB-seconds       |

#### Simple runner

```python
#!/usr/bin/env python3
import subprocess as sp
import sys


def main():
    dataset_file_path = sys.argv[1]
    output_file_path = sys.argv[2]
    mapper_file_path = sys.argv[3]
    reducer_file_path = sys.argv[4]
    with open(dataset_file_path, 'r') as in_file:
        mapper = sp.Popen([mapper_file_path], stdin=sp.PIPE, stdout=sp.PIPE, stderr=sp.PIPE, text=True)
        reducer = sp.Popen([reducer_file_path], stdin=mapper.stdout, stdout=sp.PIPE, stderr=sp.PIPE, text=True)
        for line_ind, line in enumerate(in_file):
            mapper.stdin.write(line)
            if line_ind % 1_000_000 == 0:
                print(f'{line_ind} processed')
        mapper.communicate()
        reducer_output, _ = reducer.communicate()
        with open(output_file_path, 'w') as out_file:
            out_file.write(reducer_output)


if __name__ == '__main__':
    main()
```


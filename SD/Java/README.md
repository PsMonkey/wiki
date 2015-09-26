Modifier
--------

| modifier     | class | interface | constructor | method | field |
| ------------ |:-----:|:---------:|:-----------:|:------:|:-----:|
| private      | V     |           | V           | V      | V     |
| (default)    | V(1)  | V         | V           | V      | V     |
| protected    | V(1)  |           | V           | V      | V     |
| public       | V     | V         | V           | V      | V     |
|              |       |           |             |        |       |
| static       |       |           |             | V      | V     |
| abstract     | V     | V         |             |        | V     |
| final        | V     |           |             | V      | V     |
| native       |       |           |             | V      |       |
| strictfp     | V     | V         |             | V      |       |
| synchronized |       |           |             | V      |       |
| transient    |       |           |             |        | V     |
| volatile     |       |           |             |        | V     |

(1)：只限於 inner class
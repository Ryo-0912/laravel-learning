# ctype_系

対象となる文字列を検証するときに使うメソッド


- ctype_alnum

  文字列が英数字のみで構成されているかどうか

  ```
  ctype_alnum("abc123")

  #=> true
  ```

  
- ctype_alpha

  文字列がアルファベットのみで構成されているかどうか

  ```
  ctype_alpha("HelloWorld")

  #=> true
  ```
  
- ctype_digit

  文字列が数字のみで構成されているかどうか

  ```
  ctype_digit("12345")

  #=> true
  ```
  
- ctype_upper

  文字列が大文字のアルファベットのみで構成されているかどうか

  ```
  ctype_upper("HELLO")

  #=> true
  ```
  
- ctype_lower

  文字列が小文字のアルファベットのみで構成されているかどうか

  ```
  ctype_lower("hello")

  #=> true
  ```
  
- ctype_print

  文字列が表示可能な文字（スペースを含む）で構成されているかどうか

  ```
  ctype_print("Hello World!")

  #=> true
  ```
  
- ctype_graph

  文字列が表示可能な全ての文字（スペースを除く）で構成されているかどうか

  ```
  ctype_graph("Hello@123")

  #=> true
  ```

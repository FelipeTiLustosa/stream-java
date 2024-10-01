# Stream

• É uma sequência de elementos advinda de uma fonte de dados que oferece suporte a "operações agregadas".<br>
• Fonte de dados: coleção, array, função de iteração, recurso de E/S.<br>
• Sugestão de leitura:<br>
[Stream API - Oracle](http://www.oracle.com/technetwork/pt/articles/java/streams-api-java-8-3410098-ptb.html)

## Características
• Stream é uma solução para processar sequências de dados de forma:<br>
   • Declarativa (iteração interna: escondida do programador).<br>
   • Parallel-friendly (imutável -> thread safe).<br>
   • Sem efeitos colaterais.<br>
   • Sob demanda (lazy evaluation).<br>
   • Acesso sequencial (não há índices).<br>
   • Single-use: só pode ser "usada" uma vez.<br>
   • Pipeline: operações em streams retornam novas streams. Então é possível criar uma cadeia de operações (fluxo de processamento).

## Operações intermediárias e terminais
• O pipeline é composto por zero ou mais operações intermediárias e uma terminal.<br>
• **Operação intermediária**:<br>
   • Produz uma nova stream (encadeamento).<br>
   • Só executa quando uma operação terminal é invocada (lazy evaluation).<br>
• **Operação terminal**:<br>
   • Produz um objeto não-stream (coleção ou outro).<br>
   • Determina o fim do processamento da stream.
Aqui estão exemplos claros e diretos de cada operação intermediária e terminal mencionada, explicando o que elas fazem e o que elas passam a ser quando usadas.

---

### **Operações Intermediárias**
Estas transformam uma stream em outra e são **lazy**, ou seja, não são executadas até que uma operação terminal seja chamada.

1. **`filter()`**
   - **O que faz**: Filtra os elementos da stream com base em uma condição (um **Predicate**).
   - **Exemplo**:
     ```java
     List<Integer> numbers = List.of(1, 2, 3, 4, 5);
     numbers.stream()
            .filter(n -> n % 2 == 0) // Apenas números pares
            .forEach(System.out::println); // Terminal que exibe
     // Saída: 2, 4
     ```

2. **`map()`**
   - **O que faz**: Transforma cada elemento da stream em outro (usando uma **Function**).
   - **Exemplo**:
     ```java
     List<String> names = List.of("Maria", "Alex", "Bob");
     names.stream()
          .map(String::toUpperCase) // Converte para maiúsculas
          .forEach(System.out::println); // Terminal que exibe
     // Saída: MARIA, ALEX, BOB
     ```

3. **`flatMap()`**
   - **O que faz**: Achata (flattens) uma stream de streams em uma única stream.
   - **Exemplo**:
     ```java
     List<List<String>> listOfLists = List.of(List.of("a", "b"), List.of("c", "d"));
     listOfLists.stream()
                .flatMap(List::stream) // "Achata" a lista de listas
                .forEach(System.out::println); // Terminal que exibe
     // Saída: a, b, c, d
     ```

4. **`peek()`**
   - **O que faz**: Executa uma ação para cada elemento sem modificar a stream (usado para depuração).
   - **Exemplo**:
     ```java
     List<Integer> numbers = List.of(1, 2, 3);
     numbers.stream()
            .peek(System.out::println) // Exibe sem modificar
            .map(n -> n * 2)
            .forEach(System.out::println); // Terminal que exibe
     // Saída: 1, 2, 3, 2, 4, 6
     ```

5. **`distinct()`**
   - **O que faz**: Remove elementos duplicados da stream (baseado no `equals()`).
   - **Exemplo**:
     ```java
     List<Integer> numbers = List.of(1, 2, 2, 3, 3, 3);
     numbers.stream()
            .distinct()
            .forEach(System.out::println); // Terminal que exibe
     // Saída: 1, 2, 3
     ```

6. **`sorted()`**
   - **O que faz**: Ordena os elementos da stream (usando `Comparable` ou um `Comparator`).
   - **Exemplo**:
     ```java
     List<String> names = List.of("Bob", "Alex", "Maria");
     names.stream()
          .sorted()
          .forEach(System.out::println); // Terminal que exibe
     // Saída: Alex, Bob, Maria
     ```

7. **`skip()`**
   - **O que faz**: Ignora os primeiros `n` elementos da stream.
   - **Exemplo**:
     ```java
     List<Integer> numbers = List.of(1, 2, 3, 4, 5);
     numbers.stream()
            .skip(2)
            .forEach(System.out::println); // Terminal que exibe
     // Saída: 3, 4, 5
     ```

8. **`limit()`**
   - **O que faz**: Limita o número de elementos processados pela stream a `n` elementos.
   - **Exemplo**:
     ```java
     List<Integer> numbers = List.of(1, 2, 3, 4, 5);
     numbers.stream()
            .limit(3)
            .forEach(System.out::println); // Terminal que exibe
     // Saída: 1, 2, 3
     ```

---

### **Operações Terminais**
Essas operações finalizam o pipeline de uma stream, consumindo-a e retornando um valor ou um efeito colateral.

1. **`forEach()`**
   - **O que faz**: Executa uma ação para cada elemento da stream.
   - **Exemplo**:
     ```java
     List<String> names = List.of("Maria", "Alex", "Bob");
     names.stream().forEach(System.out::println);
     // Saída: Maria, Alex, Bob
     ```

2. **`forEachOrdered()`**
   - **O que faz**: Executa uma ação para cada elemento, garantindo a ordem original de processamento.
   - **Exemplo**:
     ```java
     List<String> names = List.of("Maria", "Alex", "Bob");
     names.stream().forEachOrdered(System.out::println);
     // Saída: Maria, Alex, Bob
     ```

3. **`toArray()`**
   - **O que faz**: Converte a stream em um array.
   - **Exemplo**:
     ```java
     Object[] namesArray = names.stream().toArray();
     System.out.println(Arrays.toString(namesArray));
     // Saída: [Maria, Alex, Bob]
     ```

4. **`reduce()`**
   - **O que faz**: Reduz os elementos da stream a um único valor, combinando-os de acordo com uma operação.
   - **Exemplo**:
     ```java
     int sum = List.of(1, 2, 3).stream().reduce(0, Integer::sum);
     System.out.println(sum);
     // Saída: 6
     ```

5. **`collect()`**
   - **O que faz**: Coleta os elementos da stream em uma coleção (como `List`, `Set` ou `Map`).
   - **Exemplo**:
     ```java
     List<String> collectedNames = names.stream().collect(Collectors.toList());
     System.out.println(collectedNames);
     // Saída: [Maria, Alex, Bob]
     ```

6. **`min()`**
   - **O que faz**: Retorna o menor elemento da stream, baseado em um `Comparator`.
   - **Exemplo**:
     ```java
     Optional<Integer> min = List.of(3, 1, 4, 2).stream().min(Integer::compareTo);
     System.out.println(min.get());
     // Saída: 1
     ```

7. **`max()`**
   - **O que faz**: Retorna o maior elemento da stream, baseado em um `Comparator`.
   - **Exemplo**:
     ```java
     Optional<Integer> max = List.of(3, 1, 4, 2).stream().max(Integer::compareTo);
     System.out.println(max.get());
     // Saída: 4
     ```

8. **`count()`**
   - **O que faz**: Conta o número de elementos na stream.
   - **Exemplo**:
     ```java
     long count = names.stream().count();
     System.out.println(count);
     // Saída: 3
     ```

9. **`anyMatch()`**
   - **O que faz**: Retorna `true` se **algum** elemento da stream atender a um **Predicate**.
   - **Exemplo**:
     ```java
     boolean hasM = names.stream().anyMatch(name -> name.startsWith("M"));
     System.out.println(hasM);
     // Saída: true
     ```

10. **`allMatch()`**
    - **O que faz**: Retorna `true` se **todos** os elementos da stream atenderem a um **Predicate**.
    - **Exemplo**:
      ```java
      boolean allStartWithA = names.stream().allMatch(name -> name.startsWith("A"));
      System.out.println(allStartWithA);
      // Saída: false
      ```

11. **`noneMatch()`**
    - **O que faz**: Retorna `true` se **nenhum** elemento da stream atender a um **Predicate**.
    - **Exemplo**:
      ```java
      boolean noneStartWithZ = names.stream().noneMatch(name -> name.startsWith("Z"));
      System.out.println(noneStartWithZ);
      // Saída: true
      ```

12. **`findFirst()`**
    - **O que faz**: Retorna o primeiro elemento da stream (opcionalmente).
    - **Exemplo**:
      ```java
      Optional<String> first = names.stream().findFirst();
      System.out.println(first.get());
      // Saída: Maria
      ```

13. **`findAny()`**
    - **O que faz**: Retorna qualquer elemento da stream, útil em operações paralelas.
    - **Exemplo**:
      ```java
      Optional<String> any = names.stream().findAny();
      System.out.println(any.get());
      // Saída: Maria (ou Alex, ou Bob, dependendo da execução)
      ```

---

##

 **Short-circuit Operations**
As operações `limit()`, `anyMatch()`, `allMatch()`, `noneMatch()`, `findFirst()` e `findAny()` são chamadas de **short-circuit** porque podem interromper o processamento da stream assim que o resultado for determinado.

Exemplo:
```java
boolean hasEven = List.of(1, 3, 5, 6, 7).stream()
                 .anyMatch(n -> n % 2 == 0); // Para após encontrar o 6
// Saída: true
```
## Criar uma stream
• Basta chamar o método `stream()` ou `parallelStream()` a partir de qualquer objeto Collection.<br>
   [Documentação Collection - Oracle](https://docs.oracle.com/javase/10/docs/api/java/util/Collection.html)<br>
• Outras formas de se criar uma stream incluem:<br>
   • `Stream.of`<br>
   • `Stream.ofNullable`<br>
   • `Stream.iterate`

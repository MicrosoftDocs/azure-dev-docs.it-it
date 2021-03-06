---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 012043df3cf07de098d1a7f3a6715374814d1d9b
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507708"
---
Creare una nuova classe Java `Todo` accanto alla classe `DemoApplication` e aggiungere il codice seguente:

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

Questa classe è un modello di dominio mappato alla tabella `todo` creata in precedenza.

Per gestire tale classe, è necessario un repository. Definire una nuova interfaccia `TodoRepository` nello stesso pacchetto:

```java
package com.example.demo;

import org.springframework.data.repository.CrudRepository;

public interface TodoRepository extends CrudRepository<Todo, Long> {
}
```

Questo repository è un repository gestito da Spring Data JDBC.

Completare l'applicazione creando un controller in grado di archiviare e recuperare dati. Implementare una classe `TodoController` nello stesso pacchetto e aggiungere il codice seguente:

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Todo createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Iterable<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

Infine, arrestare l'applicazione e quindi riavviarla usando il comando seguente:

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>Test dell'applicazione

Per testare l'applicazione, è possibile usare cURL.

Creare prima di tutto un elemento "todo" nel database con il comando seguente:

```bash
curl --header "Content-Type: application/json" \
    --request POST \
    --data '{"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done": "true"}' \
    http://127.0.0.1:8080
```

Questo comando restituirà l'elemento creato, come segue:

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done":true}
```

Recuperare quindi i dati usando una nuova richiesta cURL come segue:

```bash
curl http://127.0.0.1:8080
```

Questo comando restituirà l'elenco di elementi "todo", incluso quello creato, come segue:

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up JDBC correctly!","done":true}]
```

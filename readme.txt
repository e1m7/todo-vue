
1) создаем простой html-проект

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Static Template</title>
    // вот тут...
  </head>
  <body>
    <h1>Todo Vue</h1>
  </body>
</html>

2) подключаем три css-библиотеки

  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,300italic,700,700italic">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.4.1/milligram.css">

3) подключаем библиотеку vue

  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>  

4) получаем начальное представление html-файла

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Static Template</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,300italic,700,700italic">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.4.1/milligram.css">
  </head>
  <body>
    <h1>Todo Vue</h1>
    // вот тут...
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
  </body>
</html>

5) создаем разметку приложения

    <div id="app">
      <div class="container">
        <h1>Todo List</h1>
        <ol>
          <li v-for="todo in todos">
            {{ todo }}
          </li>
        </ol>
      </div>
    </div>  

6) создадим экземпляр Vue
	а) el=элемент, который управляет компонентом на html-файле
	б) data=данные, которые есть у экземпляра


    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          todos: ["First todo string", "Second todo string"]
        }
      });
    </script>
  </body>
</html>	

7) создадим форму, чтобы пользователь мог добавлять новые строки в массив todos
	а) v-model=двусторонняя привязка
	б) внутри данных надо описать элемент todo

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Static Template</title>
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css?family=Roboto:300,300italic,700,700italic"
    />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.1/normalize.css"
    />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/milligram/1.4.1/milligram.css"
    />
  </head>
  <body>
    <h1>Todo Vue</h1>

    <div id="app">
      <div class="container">
        <h1>Todo List</h1>
        <input type="text" v-model="todo" />           // тут
        <input type="submit" value="Add" />            // тут

        <ol>
          <li v-for="todo in todos">
            {{ todo }}
          </li>
        </ol>
      </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    <script>
      new Vue({
        el: "#app",
        data: {
          todo: "",                                    // тут
          todos: ["First todo string", "Second todo string"]
        }
      });
    </script>
  </body>
</html>

8) добавим функцию обработки клика на кнопке Add

	<input type="submit" value="Add" @click="storeTodo" />

9) добавим функцию, которая обрабатывает клик

    <script>
      new Vue({
        el: "#app",

        data: {
          todo: "",
          todos: ["First todo string", "Second todo string"]
        },

        methods: {
          storeTodo() {
            this.todos.push(this.todo);
          }
        }
      });
    </script>

10) сделаем так, чтобы после добавления новой задачи поле очищалось

        methods: {
          storeTodo() {
            this.todos.push(this.todo);
            this.todo = "";
          }
        }

11) добавим две кнопки после каждой задачи
	а) редактировать
	б) удалить

        <ol>
          <li v-for="todo in todos">
            {{ todo }}
            <button @click="editTodo">Edit</button>
            <button @click="deleteTodo">Delete</button>
          </li>
        </ol>

12) в каждую функцию надо передать индекс того элемента, с которым мы будем работать

    <div id="app">
      <div class="container">
        <h1>Todo List</h1>
        <input type="text" v-model="todo" />
        <input type="submit" value="Add" @click="storeTodo" />

        <ol>
          <li v-for="(todo, index) in todos">                     // тут...
            {{ todo }}
            <button @click="editTodo(index, todo)">Edit</button>  // тут...
            <button @click="deleteTodo(index)">Delete</button>    // тут...
          </li>
        </ol>
      </div>
    </div>        

13) напишем эти две новые функции
	а) editTodo
	б) deleteTodo

        methods: {
          storeTodo() {
            this.todos.push(this.todo);
            this.todo = "";
          },
          editTodo(index, todo) {
            this.todo = todo;
          }
        }

14) пока все работает не правильно(!), мы кликаем на редактирование и в наше поле подгружается выбранная задача, надо разделить действие
	а) если редактируем
	б) если делаем новое

        <div v-if="!isEditing">
          <input type="text" v-model="todo" />
          <input type="submit" value="Add" @click="storeTodo" />
        </div>
        <div v-else>
          <input type="text" v-model="todo" />
          <input type="submit" value="Update" @click="updateTodo" />
        </div>

15) опишем метод updateTodo(), в который надо передать из метода editTodo() индекс элемента, который надо обновить, это сложно... это делается через дополнительную перменную selectedIndex


        data: {
          isEditing: false,
          selectedIndex: null,
          todo: "",
          todos: ["First todo string", "Second todo string"]
        },

16) опишем функцию updateTodo(), которая использует функцию splice() склейку и заменит старый элемент массива на новый

        methods: {
          storeTodo() {
            this.todos.push(this.todo);
            this.todo = "";
          },
          editTodo(index, todo) {
            this.todo = todo;
            this.selectedIndex = index;
            this.isEditing = true;
          },
          updateTodo() {
            this.todos.splice(this.selectedIndex, 1, this.todo);
            this.isEditing = false;
          }
        }

17) добавим метод deleteTodo()

          deleteTodo(index) {
            this.todos.splice(index, 1);
          }
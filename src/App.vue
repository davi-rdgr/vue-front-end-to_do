<script setup>
import { onMounted, ref } from "vue";

// recupera os dados salvos no local storage e adiciona nos array to_do e done
onMounted(() => {
  arrayTask.value = JSON.parse(localStorage.getItem("to_do")) || [];
  arrayExclude.value = JSON.parse(localStorage.getItem("done")) || [];
});

//arrays para guardar to-do e done
let arrayTask = ref([]);
let arrayExclude = ref([]);

/* input submit que quando clicado, lê o conteúdo do input 'main_input_task', faz um tratamento de dados e, se verdadeiro, executa a função adicionarTask(); */
const adicionarTaskSubmit = (e) => {
  e.preventDefault();

  /* tratamento de dados para que não possa ser criado cards vazios */
  let task = document.querySelector(".main_input_task");
  if (task.value === "") {
    console.log("Valores vazios!");
    alert("Tente adicionar uma mensagem! ;)");
    return;
  }
  adicionarTask();
};

/* cria e adiciona uma nova task no array to-do e chama a função para atualizar o local storage */
const adicionarTask = () => {
  // mudar de posição depois:
  let task = document.querySelector(".main_input_task");
  arrayTask.value.push(task.value);
  task.value = "";

  salvarLocalStorage();
};

/* altera o array da task de to-do para done e chama a função para atualizar o local storage */
const alterarDoneList = (index, task) => {
  arrayExclude.value.push(task);
  arrayTask.value.splice(index, 1);

  salvarLocalStorage();
};

/* exclui permanentemente a task do array to-do selecionando por índice e chama a função para atualizar o local storage */
const alternarExcluirList = (index) => {
  arrayExclude.value.splice(index, 1);

  salvarLocalStorage();
};
// função que atualiza o local storage baseado nos array to-do e done sempre que chamada.
const salvarLocalStorage = () => {
  localStorage.setItem("to_do", JSON.stringify(arrayTask.value));
  localStorage.setItem("done", JSON.stringify(arrayExclude.value));
};

// BOTÕES

// melhorar:
let boolean = null;
const toggleMenu = () => {
  let side_menu = document.querySelector(".side_menu");
  let burguer = document.querySelectorAll(".burguer");
  let button = document.querySelector(".buttonnone");

  if (boolean === null || boolean !== true) {
    side_menu.style.transform = "translateX(0%)";
    button.style.backgroundColor = "rgba(32, 61, 77, 0.925)";
    burguer.forEach((element) => {
      element.style.background = "#ffffff";
    });

    boolean = true;
  } else if (boolean === true) {
    side_menu.style.transform = "translateX(-100%)";

    button.style.backgroundColor = "#ffffff";
    burguer.forEach((element) => {
      element.style.background = "#203D4D";
    });

    boolean = false;
  }
};

// botoes menu

const toggleCollor = () => {};
const see_all = () => {
  let todo_list_sec = document.querySelector(".todo_list_sec");
  let dolist_sec = document.querySelector(".dolist_sec");
  dolist_sec.style.display = "block";
  todo_list_sec.style.display = "block";
};
const only_todo = () => {
  let todo_list_sec = document.querySelector(".todo_list_sec");
  let dolist_sec = document.querySelector(".dolist_sec");
  dolist_sec.style.display = "none";
  todo_list_sec.style.display = "block";
};
const only_done = () => {
  let todo_list_sec = document.querySelector(".todo_list_sec");
  let dolist_sec = document.querySelector(".dolist_sec");
  dolist_sec.style.display = "block";
  todo_list_sec.style.display = "none";
};
</script>

<template>
  <div class="side_menu">
    <nav>
      <button class="nav_menu nav_menu_color_mode" v-on:click="toggleColor()">
        Dark - Light mode
      </button>
      <button class="nav_menu nav_menu_see_all" v-on:click="see_all()">Ver todas</button>
      <button class="nav_menu nav_menu_only_todo" v-on:click="only_todo()">
        Apenas to-do
      </button>
      <button class="nav_menu nav_menu_only_done" v-on:click="only_done()">
        Apenas done
      </button>
    </nav>
  </div>
  <header>
    <section class="header_sec">
      <div class="burguer_button_content">
        <button class="buttonnone" v-on:click="toggleMenu()">
          <div class="burguer"></div>
          <div class="burguer"></div>
          <div class="burguer"></div>
        </button>
      </div>

      <h1>Make your to-do list!</h1>
      <div class="space_content"></div>
    </section>
  </header>
  <main>
    <section class="form_sec">
      <form @submit="adicionarTaskSubmit" method="post">
        <div class="main_form_style">
          <input
            class="main_input_task"
            type="text"
            name="tarefa"
            placeholder="Qual será sua tarefa ?"
          />
          <input class="main_input_submit" type="submit" value="Adicionar!" />
        </div>
      </form>
    </section>

    <section class="todo_list_sec">
      <div class="todo_content">
        <h2>To do list</h2>
        <section class="todo_lists">
          <article v-for="(task, index) in arrayTask" :key="index" class="todo_article">
            <ul>
              <li>{{ task }}</li>
              <div class="task_list_check" @click="alterarDoneList(index, task)">
                <img src="../src/assets/svg/image2.svg" alt="" />
              </div>
            </ul>
          </article>
        </section>
      </div>
    </section>
    <section class="dolist_sec">
      <div class="do_content">
        <h2>Done list</h2>
        <section class="do_lists">
          <article
            v-for="(exclude, index) in arrayExclude"
            :key="index"
            class="do_article"
          >
            <ul>
              <li>{{ exclude }}</li>
              <div class="task_list_check" @click="alternarExcluirList(index)">
                <img src="../src/assets/svg/image1.svg" alt="" />
              </div>
            </ul>
          </article>
        </section>
      </div>
    </section>
  </main>
</template>

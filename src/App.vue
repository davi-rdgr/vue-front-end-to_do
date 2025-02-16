<script setup>
import { onMounted, ref } from "vue";


import { variavel } from "../src/Buttons.vue"
/* console.log(variavel); */


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
let boolean = false;

const toggleMenu = () => {
  let side_menu = document.querySelector(".side_menu");

  if (boolean !== true) {
    side_menu.style.transform = "translateX(0%)";
    boolean = true;

  } else if (boolean === true) {
    side_menu.style.transform = "translateX(-100%)";
    boolean = false;

  }
};

// botoes menu
let verDo = ref(true);
let verDone = ref(true);
let mudarCor = ref(true);

const toggleColor = () => {
  mudarCor.value = !mudarCor.value;
  let side_menu = document.querySelector(".side_menu");
  side_menu.style.transform = "translateX(-100%)";
  boolean = false;
};
const see_all = () => {
  verDo.value = true;
  verDone.value = true;
  let side_menu = document.querySelector(".side_menu");
  side_menu.style.transform = "translateX(-100%)";
  boolean = false;
};
const only_todo = () => {
  verDo.value = true;
  verDone.value = false;
  let side_menu = document.querySelector(".side_menu");
  side_menu.style.transform = "translateX(-100%)";
  boolean = false;
};
const only_done = () => {
  verDo.value = false;
  verDone.value = true;
  let side_menu = document.querySelector(".side_menu");
  side_menu.style.transform = "translateX(-100%)";
  boolean = false;
};


let boolean_menu = true;
if (mudarCor && boolean) {
  boolean_menu = false;
} else {
  boolean_menu = true;
}

</script>

<template>
  <div :class="mudarCor ? 'dark_mode_container' : 'white_mode_container'">
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
          <button :class="mudarCor ? 'dark_mode_button' : 'white_mode_button'" v-on:click="toggleMenu()">
            <div :class="mudarCor ? 'dark_mode_burguer' : 'white_mode_burguer'"></div>
            <div :class="mudarCor ? 'dark_mode_burguer' : 'white_mode_burguer'"></div>
            <div :class="mudarCor ? 'dark_mode_burguer' : 'white_mode_burguer'"></div>
          </button>
        </div>

        <h1 :class="mudarCor ? 'dark_mode_h1' : 'white_mode_h1'">Make your to-do list!</h1>
        <nav class="header_menu_desk">
          <ul>
            <li><button :class="mudarCor ? 'nav_menu_desk_dark' : 'nav_menu_desk_white'" v-on:click="see_all()">See
                all</button></li>
            <li><button :class="mudarCor ? 'nav_menu_desk_dark' : 'nav_menu_desk_white'" v-on:click="only_todo()">See to
                do</button></li>
            <li><button :class="mudarCor ? 'nav_menu_desk_dark' : 'nav_menu_desk_white'" v-on:click="only_done()">See
                done</button></li>
            <li><button :class="mudarCor ? 'nav_menu_desk_dark' : 'nav_menu_desk_white'" v-on:click="toggleColor()">Dark
                mode</button></li>
          </ul>
        </nav>
        <div class="space_content"></div>
      </section>
    </header>
    <main>
      <section class="form_sec">
        <form @submit="adicionarTaskSubmit" method="post">
          <div class="main_form_style">
            <input class="main_input_task" :class="mudarCor ? 'dark_mode_input_search' : 'white_mode_input_search'"
              type="text" name="tarefa" placeholder="Qual será sua tarefa ?" />
            <input class="main_input_submit" :class="mudarCor ? 'dark_mode_input_submit' : 'white_mode_input_submit'"
              type="submit" value="Adicionar!" />
          </div>
        </form>
      </section>

      <section v-if="verDo === true" class="todo_list_sec">
        <div class="todo_content">
          <h2 :class="mudarCor ? 'dark_mode_h2' : 'white_mode_h2'">To do list</h2>
          <section class="todo_lists">
            <article v-for="(task, index) in arrayTask" :key="index" class="todo_article">
              <ul>
                <li :class="mudarCor ? 'dark_mode_p' : 'white_mode_p'">{{ task }}</li>
                <div class="task_list_check" @click="alterarDoneList(index, task)">
                  <img src="../src/assets/svg/darkmode_image2.svg" alt="" />
                </div>
              </ul>
            </article>
          </section>
        </div>
      </section>
      <section :class="mudarCor ? 'dark_mode' : 'white_mode'" v-if="verDone === true" class="dolist_sec">
        <div class="done_content">
          <h2 :class="mudarCor ? 'dark_mode_h2' : 'white_mode_h2'">Done list</h2>
          <section class="done_lists">
            <article v-for="(exclude, index) in arrayExclude" :key="index" class="done_article">
              <ul>
                <li :class="mudarCor ? 'dark_mode_p' : 'white_mode_p'">{{ exclude }}</li>
                <div class="task_list_check" @click="alternarExcluirList(index)">
                  <img src="../src/assets/svg/darkmode_image1.svg" alt="" />
                </div>
              </ul>
            </article>
          </section>
        </div>
      </section>
    </main>
  </div>
</template>

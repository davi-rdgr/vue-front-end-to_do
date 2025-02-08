<script setup>
import { ref } from 'vue';

//array
let arrayTask = ref([]);
let arrayExclude = ref(["ta excluído"]);

const adicionarTaskSubmit = (e) => {
  e.preventDefault();

  adicionarTask();

}
// função para adicionar uma task
const adicionarTask = () => {
  let task = document.querySelector('.main_input_task');

  arrayTask.value.push(task.value);
  task.value = "";
}

</script>

<template>

  <header>
    <section class="header_sec">
      <div class="burguer_button_content">
        <div class="burguer"></div>
        <div class="burguer"></div>
        <div class="burguer"></div>
      </div>

      <h1>Make your to-do list!</h1>
      <div class="space_content"></div>
    </section>
  </header>
  <main>
    <section class="form_sec">
      <form @submit="adicionarTaskSubmit" method="post">
        <div class="main_form_style">
          <input class="main_input_task" type="text" name="tarefa" placeholder="Qual será sua tarefa ?">
          <input class="main_input_submit" type="submit" value="Adicionar!">
        </div>
      </form>
    </section>

    <section class="todo_list_sec">
      <div class="todo_content">
        <h2>To do list</h2>
        <section class="todo_lists">
          <article class="todo_article">
            <ul>
              <li>Lembrar de tirar o lixo amanhã</li>
            </ul>
          </article>
          <article v-for="task in arrayTask" class="todo_article">
            <ul>
              <li> {{ task }}</li>
            </ul>
          </article>
        </section>
      </div>
    </section>
    <section class="dolist_sec">
      <div class="do_content">
        <h2>Done list</h2>
        <section class="do_lists">
          <article class="do_article">
            <ul>
              <li>Amanhã preciso passear com o cachorro, mas não posso esquecer que preciso levar uma
                sacola
                caso o precioso faça suas necessidades na rua.</li>
            </ul>
          </article>
          <article v-for="exclude in arrayExclude" class="do_article">
            <ul>
              <li>{{ exclude }}</li>
            </ul>
          </article>
        </section>
      </div>
    </section>

  </main>

</template>
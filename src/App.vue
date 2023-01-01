<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Greet from "./components/Greet.vue";
import {ref} from "vue";
import {invoke} from "@tauri-apps/api/tauri";
import {Configuration, OpenAIApi} from "openai";
const greetMsg = ref("");
const name = ref("");

async function greet() {
  greetMsg.value = await invoke("greet", { name: name.value });
}

//watch the search input
const search = ref("");
const theAnswer = ref("");

//function to search using api
async function searchApi() {
  document.getElementById("answerModalButton").click();
  const { Configuration, OpenAIApi } = require("openai");
  const configuration = new Configuration({
    apiKey: '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'
  });

  const test = await fetch("https://api.openai.com/v1/engines/davinci/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      prompt: search.value,
      max_tokens: 5,
      temperature: 0.9,
      top_p: 1,
      frequency_penalty: 0,
      presence_penalty: 0,
      stop: ["\n", " Human:", " AI:"]
    }),
  });
  const data = await test.json();
  console.log(data);

  const openai = new OpenAIApi(configuration);
  const completion = openai.createCompletion({
    model: "text-davinci-003",
    prompt: args.question,
    max_tokens: 1000,
  });

  await completion.then(async (r) => {
    theAnswer.value = await r.data.choices[0].text;
     await  invoke("console_log", { message: theAnswer.value });
  });

  return theAnswer;
}

</script>

<template>
  <div class="container d-flex justify-content-center" style="height: 100vh;">
    <!-- google search bar -->
    <div class="row top-20">
      <div class="col-12">
        <!-- search motor logo -->
        <div class="row mb-3">
          <div class="col-12">
            <img src="/logo.png" alt="logo" class="img-fluid" style="width: 60px; height: 60px; border-radius: 50%;">
          </div>
        </div>
        <div class="input-group mb-3">
          <input type="text" class="form-control" placeholder="Recherche une technologie" aria-label="Recherche" aria-describedby="button-addon2" name="search" v-model="search">
          <button class="btn btn-sm btn-info" type="button" id="button-addon2" @click="searchApi">Rechercher</button>
        </div>
        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#answerModal" ref="answerModalButton" id="answerModalButton" style="display: none;">
          Launch demo modal
        </button>
      </div>
    </div>
    <div class="accordion" id="accordionExample">
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingThree">
          <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#collapseThree " aria-expanded="false" aria-controls="collapseThree">
            React Native
          </button>
        </h2>
        <div id="collapseThree" class="accordion-collapse collapse" aria-labelledby="headingThree" data-bs-parent="#accordionExample">
          <div class="accordion-body">
            <!--Passage about React Native framework-->
            <p>
              <a href="https://reactnative.dev/" class="m-auto">
                <img src="https://reactnative.dev/img/header_logo.svg" alt="React Native logo" class="img-fluid" style="width: 100px; height: 100px;">
              </a>
              React Native is an open-source mobile application framework created by Facebook, Inc. It is used to develop applications for Android, Android TV, iOS, macOS, tvOS, Web, Windows and UWP by enabling developers to use React along with native platform capabilities. React Native is a framework for building native apps using React. It uses the same design as React, letting you compose a rich mobile UI from declarative components.
            </p>
          </div>
        </div>
      </div>
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingOne">
          <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
           Tauri
          </button>
        </h2>
        <div id="collapseOne" class="accordion-collapse collapse show" aria-labelledby="headingOne" data-bs-parent="#accordionExample">
          <div class="accordion-body">
            <!--Passage about Tauri framework-->
            <p>
              <!--logo from internet and link to the website-->
              <a href="https://tauri.studio/en/" target="_blank">
                <img src="https://tauri.studio/img/logo.svg" alt="logo" class="img-fluid" style="width: 60px; height: 60px; border-radius: 50%;">
              </a>
              Tauri is a framework for building cross-platform desktop applications with web technologies. It is a tool that allows you to build a desktop application using web technologies (HTML, CSS, and JavaScript) and then compile it to a native binary. It is a tool that allows you to build a desktop application using web technologies (HTML, CSS, and JavaScript) and then compile it to a native binary.
            </p>
          </div>
        </div>
      </div>
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingTwo">
          <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
            Flutter
          </button>
        </h2>
        <div id="collapseTwo" class="accordion-collapse collapse" aria-labelledby="headingTwo" data-bs-parent="#accordionExample">
          <div class="accordion-body">
            <!--Passage about Flutter framework-->
            <p>
              <a href="https://flutter.dev/" class="m-auto">
                <img src="https://flutter.dev/images/flutter-logo-sharing.png" alt="Flutter logo" class="img-fluid" style="width: 100px; height: 100px;">
              </a>
              Flutter is an open-source UI software development kit created by Google. It is used to develop applications for Android, iOS, Linux, Mac, Windows, Google Fuchsia, and the web from a single codebase. The first version of Flutter was known as codename "Sky" and ran on the Android operating system.
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!--results accordion-->

  <!--modal to show the answers-->
  <div class="modal fade modal-xl" id="answerModal" tabindex="-1" aria-labelledby="answerModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-dialog-scrollable">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="answerModalLabel">Réponse</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body text-center">
          <!--comparative table of technologies-->
          <table class="table table-responsive table-striped table-hover">
            <thead>
              <tr>
                <th scope="col">Technologies</th>
                <th scope="col">Tauri</th>
                <th scope="col">Flutter</th>
                <th scope="col">React Native</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th scope="row">Langage</th>
                <td>HTML, CSS, JS</td>
                <td>Dart</td>
                <td>JS</td>
              </tr>
              <tr>
                <th scope="row">OS</th>
                <td>Windows, Linux, Mac</td>
                <td>Android, iOS, Linux, Mac, Windows, Web</td>
                <td>Android, iOS, Linux, Mac, Windows, Web</td>
              </tr>
              <tr>
                <th scope="row">Licence</th>
                <td>MIT</td>
                <td>BSD</td>
                <td>MIT</td>
              </tr>
              <tr>
                <th scope="row">Langage natif</th>
                <td>Oui</td>
                <td>Oui</td>
                <td>Non</td>
              </tr>
              <tr>
                <th scope="row">Performance</th>
                <td>Très bonne</td>
                <td>Très bonne</td>
                <td>Moyenne</td>
              </tr>
              <tr>
                <th scope="row">Mise à jour</th>
                <td>Très bonne</td>
                <td>Très bonne</td>
                <td>Moyenne</td>
              </tr>
              <tr>
                <th scope="row">Documentation</th>
                <td>Très bonne</td>
                <td>Très bonne</td>
                <td>Moyenne</td>
              </tr>
              <tr>
                <th scope="row">Communauté</th>
                <td>Très bonne</td>
                <td>Très bonne</td>
                <td>Moyenne</td>
              </tr>a
              <tr>
                <th scope="row">Sécurité</th>
                <td>Très bonne</td>
                <td>Très bonne</td>
                <td>Moyenne</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fermer</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo.vite:hover {
  filter: drop-shadow(0 0 2em #747bff);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #249b73);
}
</style>

<template>
  <div class="formulaire">
    <form novalidate class="md-layout" encType="multipart/form-data">
      <md-card class="md-layout-item md-size-50 md-small-size-100">
        <md-card-content>
          <md-field>
            <label for="title">Titre</label>
            <md-input
              type="text"
              class="form-control"
              id="title"
              v-model="title"
              max-length="50"
              required
            />
          </md-field>

          <md-field>
            <label for="body">Contenu</label>
            <md-textarea
              class="form-control"
              id="body"
              v-model="body"
              rows="5"
              cols="33"
              required
              md-autogrow
            ></md-textarea>
          </md-field>

          <md-field>
            <label for="image_URL"></label>
            <md-input
              type="file"
              class="form-control"
              id="image_URL"
              @change="setSelectedImage"
            />
          </md-field>
        </md-card-content>

        <md-card-actions>
          <md-button
            type="submit"
            class="btnValidation"
            v-on:click.prevent="createPost"
            >Partager</md-button
          >
        </md-card-actions>
      </md-card>
    </form>
  </div>
</template>

<script>
export default {
  name: "CreatePostForm",
  data() {
    return {
      title: "",
      body: "",
      selectedImage: "NULL",
    };
  },
  methods: {
    setSelectedImage(event) {
      this.selectedImage = event.target.files[0];
    },
    createPost() {
      let idUser = parseInt(localStorage.getItem("Id"));
      let dataForm = { userId: idUser, title: this.title, body: this.body };
      let jsonDataForm = JSON.stringify(dataForm);
      let formData = new FormData();
      formData.append("image", this.selectedImage);
      formData.append("post", jsonDataForm);
      console.log(dataForm);

      async function postForm(dataToSend) {
        try {
          let response = await fetch("http://localhost:3000/api/posts/", {
            method: "POST",
            headers: {
              authorization: "Bearer " + localStorage.getItem("token"),
            },
            body: dataToSend,
          });
          if (response.ok) {
            let responseId = await response.json();
            window.location.href = " http://localhost:8080/signup#/allpost";
            console.log(responseId);
            location.reload();
          } else {
            console.error("Retour du serveur : ", response.status);
          }
        } catch (e) {
          console.log(e);
        }
      }
      postForm(formData);
    },
  },
};
</script>

<style scoped lang="scss">
.btnValidation {
  background-color:#1e81b0 ;
  color: #f0e8c7;
  font-weight: bold;box-shadow: 10px 5px 5px grey;
}
form {
  width: 200%;
}
.md-card{
  width: 500px;
  box-shadow: 10px 5px 5px grey;
  height: 250px;
}
.md-layout {
  margin-bottom: 50px;
  margin-top: 50px;
}

@media all and (max-width: 899px) {
  form {
    width: 100%;
  }
}

@media all and (max-width: 599px) {
  form {
    width: 100%;
  }
}
</style>

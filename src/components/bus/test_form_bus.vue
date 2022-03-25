<template>
  <div>
    <div class="container">
      <!-- <button @click="handleSubmit">aazazaz</button> -->
      <b-form @submit="onSubmit" @reset="onReset" v-if="show">
        <b-form-group
          id="input-group-1"
          label="Email address:"
          label-for="input-1"
          description="We'll never share your email with anyone else."
        >
          <b-form-input
            id="input-1"
            v-model="form_e.email"
            type="email"
            placeholder="Enter email"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-2"
          label="Your password:"
          label-for="input-2"
        >
          <b-form-input
            id="input-2"
            type="password"
            v-model="form_e.password"
            placeholder="Enter password"
            required
          ></b-form-input>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
      <b-card class="mt-3" header="Form Data Result">
        <pre class="m-0">{{ form_e }}</pre>
      </b-card>
    </div>
  </div>
</template>

<script setup>
const form_e = { email: "", password: "" };
const show = true;
let tokentoken;
let refreshrefresh;

function onSubmit() {
  // preventDefault();
  alert(JSON.stringify(form_e));
  handleSubmit();
}

function onReset() {
  this.form_e.email = "";
  this.form_e.password = "";
  this.show = false;
  this.$nextTick(() => {
    this.show = true;
  });
}

async function handleSubmit() {
  console.log("handle");
  console.log({ form_e });

  let response = await fetch("https://apibus.quidam.re/api/login_check", {
    method: "POST",
    body: JSON.stringify({
      username: form_e.email,
      password: form_e.password,
    }),
    headers: {
      Accept: "application/json",
      "Content-type": "application/json; charset=UTF-8",
    },
  })
    .then((res) => res.json())
    .catch((err) => err);

  console.log(response.token);

  tokentoken = response.token;
  refreshrefresh = response.refresh_token;
  console.log({ tokentoken });
  console.log({ refreshrefresh });
  submit_stuff(tokentoken, refreshrefresh);
}

async function submit_stuff() {
  console.log("submit_stuff");
  console.log({ form_e });
  console.log(tokentoken);
  console.log({ refreshrefresh });

  let response_bus = await fetch("https://apibus.quidam.re/api/buses/1", {
    method: "GET",
    headers: {
      Authorization: `Bearer ${tokentoken}`,
    },
  })
    .then((res) => res.json())
    .catch((err) => err);

  console.log(response_bus);
}

// export default {
//   data() {
//     return {
//       form: {
//         email: "",
//         password: "",
//       },
//       show: true,
//     };
//   },
//   methods: {
//     onSubmit(event) {
//       event.preventDefault();
//       alert(JSON.stringify(this.form));
//     },
//     onReset(event) {
//       event.preventDefault();
//       // Reset our form values
//       this.form.email = "";
//       this.form.password = ""; // Trick to reset/clear native browser form validation state
//       this.show = false;
//       this.$nextTick(() => {
//         this.show = true;
//       });
//     },
//     async work_api() {
//       const response = await fetch("https://apibus.quidam.re/api/login_check");

//       const answer = await response.json();
//       console.log(answer);
//       // const { data: users } = await response.json();

//       // this.users = users;
//     },

//     async handleSubmit() {
//       console.log("handle");

//       let laform = new FormData();
//       laform.append(form.email);
//       laform.append(form.password);

//       console.log({ laform });

//       let response = await fetch("https://apibus.quidam.re/api/login_check", {
//         method: "POST",
//         body: laform,
//       })
//         .then((res) => res.json())
//         .catch((err) => err);

//       if (response.message === "") {
//         this.notification = "Article ajouter avec succès";
//         this.new_article = response.article_id;
//       }
//     },
//   },
// };
</script>

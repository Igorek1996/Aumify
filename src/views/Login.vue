<template>
  <main class="main">
    <v-form @submit.prevent="validate()" class="form">
      <v-container>
        <v-progress-linear
          :active="loading"
          :indeterminate="loading"
          color="success"
          striped
          height="5"
        />
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="v$.email.$model"
              label="E-mail"
              variant="solo"
              :error="v$.email.$error"
              :error-messages="getErrorMessages('email')"
            />
          </v-col>

          <v-col cols="12">
            <v-text-field
              v-model="v$.password.$model"
              label="Password"
              variant="solo"
              :error="v$.password.$error"
              :error-messages="getErrorMessages('password')"
            />
          </v-col>

          <v-col cols="12">
            <v-btn type="submit" color="success"> Submit </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter } from "vue-router";
import { useVuelidate } from "@vuelidate/core";
import { required, helpers, email, minLength } from "@vuelidate/validators";

const router = useRouter();

const loading = ref(false);

const passwordMinLength = ref(6);
const form = ref({
  email: "",
  password: "",
});

const rules = {
  email: {
    required: helpers.withMessage("Field is required", required),
    email: helpers.withMessage("Email is invalid", email),
  },
  password: {
    required: helpers.withMessage("Field is required", required),
    minLength: helpers.withMessage(
      `Length should be more than ${passwordMinLength.value}`,
      minLength(passwordMinLength.value)
    ),
  },
};

const v$ = useVuelidate(rules, form);

const getErrorMessages = (field) => {
  const fieldErrors = v$.value[field].$errors;
  return fieldErrors.map((error) => error.$message);
};

const validate = async () => {
  const result = await v$.value.$validate();

  if (!result) {
    return;
  }

  loading.value = true;
  await new Promise((resolve) => setTimeout(resolve, 2000));
  form.value = {
    email: "",
    password: "",
  };
  v$.value.$reset();
  loading.value = false;
  router.push("/game");
};
</script>

<style>
.form {
  margin: 100px auto 0 auto;
}
</style>
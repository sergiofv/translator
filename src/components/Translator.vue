<template>
  <div>
    <h4>Simple DeepL API integration</h4>
    <textarea
      v-model="fromText"
      placeholder="Enter your text here"
      cols="50"
      rows="10"
    ></textarea>
  </div>
  <br />
  <div>
    <select v-model="toLanguage">
      <option
        v-for="{ prefix, value, key } in languageOptions"
        :value="prefix"
        :key="key"
      >
        {{ value }}
      </option>
    </select>
    <button @click="translate">Translate</button>
  </div>
  <br />
  <div>
    <textarea :value="translatedText" cols="50" rows="10"></textarea>
  </div>
</template>

<script>
export default {
  data() {
    return {
      fromText: "",
      translatedText: "",
      toLanguage: "",
      languageOptions: [
        { prefix: "", value: "Select language" },
        { prefix: "BG", value: "Bulgarian" },
        { prefix: "CS", value: "Czech" },
        { prefix: "DA", value: "Danish" },
        { prefix: "DE", value: "German" },
        { prefix: "EL", value: "Greek" },
        { prefix: "EN-GB", value: "English (British)" },
        { prefix: "EN-US", value: "English (American)" },
        { prefix: "ES", value: "Spanish" },
        { prefix: "ET", value: "Estonian" },
        { prefix: "FI", value: "Finnish" },
        { prefix: "FR", value: "French" },
        { prefix: "HU", value: "Hungarian" },
        { prefix: "IT", value: "Italian" },
        { prefix: "JA", value: "Japanese" },
        { prefix: "LT", value: "Lithuanian" },
        { prefix: "LV", value: "Latvian" },
        { prefix: "NL", value: "Dutch" },
        { prefix: "PL", value: "Polish" },
        { prefix: "PT-PT", value: "Portuguese" },
        { prefix: "PT-BR", value: "Portuguese (Brazilian)" },
        { prefix: "RO", value: "Romanian" },
        { prefix: "RU", value: "Russian" },
        { prefix: "SK", value: "Slovak" },
        { prefix: "SL", value: "Slovenian" },
        { prefix: "SV", value: "Swedish" },
        { prefix: "ZH", value: "Chinese" },
      ],
    };
  },
  methods: {
    translate() {
      if (this.toLanguage === "") {
        alert("Please select a language!");
        return;
      } else if (this.fromText === "") {
        alert("Please enter some text!");
        return;
      } else if (this.fromText.length > 4000) {
        alert("The text cannot be longer than 4000 characters!");
        return;
      }
      fetch(
        `https://api-free.deepl.com/v2/translate?auth_key=${process.env.VUE_APP_AUTH_KEY}&text=${this.fromText}&target_lang=${this.toLanguage}`
      )
        .then((res) => res.json())
        .then((data) => (this.translatedText = data.translations[0].text))
        .catch((error) => (console.log(error), alert("Error!")));
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

select {
  margin: 1em;
}
</style>

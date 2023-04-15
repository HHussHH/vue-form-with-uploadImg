<script setup>
import { reactive, computed } from "vue";
import axios from "axios";
const review = reactive({
  author: "",
  stars: null,
  text: "",
  photo: null,
  isRecommended: true,
});

const previewFilePath = computed(() => {
  if (review.photo) return URL.createObjectURL(review.photo);

  return "#";
});

const starts = [1, 2, 3, 4, 5];

const submit = () => {
  console.log("sumbit!");

  axios
    .post("/api/review", review, {
      headers: {
        "Content-Type": "multipart/form-data",
      },
    })
    .then((res) => {
      console.log(res);
    })
    .catch((err) => {
      console.log(err);
    })
    .finally(() => {
      console.log("Конец!");
    });
};

const uploadFile = (e) => {
  const [file] = e.target.files;
  review.photo = file;
};
</script>

<template>
  <form class="container pt-5 pb-5" @submit.prevent.stop="submit">
    <input
      class="form-control mb-3"
      type="text"
      v-model="review.author"
      placeholder="Как вас зовут"
    />
    <!--/.form-container-->
    <textarea
      rows="3"
      class="form-control mb-3"
      v-model="review.text"
      placeholder="Что понравилось, а что нет?"
    ></textarea>
    <!--/.form-control-->
    <h4>Оценка</h4>
    <div v-for="star in starts" :key="star" class="form-check">
      <input
        class="form-check-input"
        type="checkbox"
        v-model="review.stars"
        :true-value="star"
        :false-value="null"
        :id="`star${star}`"
      />
      <label class="form-check-label" :for="`star${star}`"> {{ star }}</label>
    </div>
    <!--/.form-check-->
    <div class="mb-3 mt-3">
      <label class="form-label">Фото</label>
      <input class="form-control" type="file" @change="uploadFile" />

      <img :src="previewFilePath" alt="" class="w-100" mt-2 />
    </div>
    <!--mb-3 mt-3-->
    <div class="form-check">
      <input
        class="form-check-input"
        type="radio"
        id="adv1"
        v-model="review.isRecommended"
        :value="true"
      />
      <label class="form-check-label" for="adv1"> Советую </label>
    </div>
    <!--/.form-check-->
    <div class="form-check">
      <input
        class="form-check-input"
        type="radio"
        id="adv2"
        v-model="review.isRecommended"
        :value="false"
      />
      <label class="form-check-label" for="adv2"> Не советую </label>
    </div>

    <!--/.form-check-->

    <button class="btn btn-primary mt-4">Отправить.</button>
  </form>
</template>

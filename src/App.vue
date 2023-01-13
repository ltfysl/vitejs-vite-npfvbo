<template>
  <v-card max-width="80%" class="mx-auto">
    <v-card-title>A Simple Imagee Classifier</v-card-title><br />
    <input type="file" @change="onImageChange" />

    <img v-if="imgURL" :src="imgURL" />
  </v-card>
</template>

<script>
import * as ml5 from 'ml5';

export default {
  data: () => ({
    imgFile: null,
    imgURL: null,
    results: null,
    loading: false,
  }),
  created() {
    this.classifier = ml5.imageClassifier('MobileNet', this.modelLoaded);
  },
  methods: {
    modelLoaded() {
      console.log('Model loaded!');
    },
    predict() {
      this.loading = true;
      const img = document.createElement('img');
      console.log(img);
      img.src = this.imgURL;
      console.log(img.naturalWidth);
      img.height = 500;
      img.width = 500;
      console.log(img);
      this.classifier.predict(img, 5, (err, res) => {
        if (!err) {
          this.loading = false;
          console.log(res);
          this.results = res;
        }
      });
    },
    onImageChange(e) {
      this.imgFile = e.target.files[0];
      console.log('hello');
      this.results = null;
      if (this.imgFile !== null) {
        this.imgURL = URL.createObjectURL(this.imgFile);
        this.predict();
      } else {
        this.imgURL = null;
      }
    },
  },
};
</script>

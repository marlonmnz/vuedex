<template>
  <div
    id="card"
    class="
      border
      rounded
      border-slate-300
      m-2
      flex flex-col
      items-center
      w-80
      max-w-xs
    "
  >
    <div class="">
      <div>
        <figure>
          <img :src="currentImg" alt="Placeholder image" />
        </figure>
      </div>
      <div class="flex flex-col items-center">
        <div class="flex flex-col items-center">
          <p class="font-bold uppercase">{{ index }}. {{ name }}</p>
          <p>{{ pokemon.type }}</p>
        </div>
        <div class="content">
          <button
            class="
              my-2
              border border-green-500
              rounded
              hover:bg-green-500 hover:text-slate-50
              w-28
              py-1
            "
            @click="changeSprite"
          >
            Mudar Sprite
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import api from "../services/api";
export default {
  name: "pokemon",
  props: {
    index: Number,
    name: String,
    url: String,
  },
  created: function () {
    api.get(this.url).then((response) => {
      this.pokemon.type = response.data.types[0].type.name;
      this.pokemon.front = response.data.sprites.front_default;
      this.pokemon.back = response.data.sprites.back_default;
      this.currentImg = response.data.sprites.front_default;
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
      },
    };
  },
  methods: {
    changeSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
  },
};
</script>

<template>
  <div>
    <div class="relative py-16 bg-white">
      <div
        class="hidden absolute top-0 inset-x-0 h-1/2 bg-gray-50 lg:block"
        aria-hidden="true"
      />
      <div class="max-w-7xl mx-auto bg-indigo-600 lg:bg-transparent lg:px-8">
        <div class="lg:grid lg:grid-cols-12">
          <div
            class="
              relative
              z-10
              lg:col-start-1
              lg:row-start-1
              lg:col-span-4
              lg:py-16
              lg:bg-transparent
            "
          >
            <div
              class="absolute inset-x-0 h-1/2 bg-gray-50 lg:hidden"
              aria-hidden="true"
            />
            <div
              class="
                max-w-md
                mx-auto
                px-4
                sm:max-w-3xl sm:px-6
                lg:max-w-none lg:p-0
              "
            >
              <div
                class="
                  aspect-w-10 aspect-h-6
                  sm:aspect-w-2 sm:aspect-h-1
                  lg:aspect-w-1
                "
              >
                <img
                  class="object-cover object-center rounded-3xl shadow-2xl"
                  :src="imageUrl"
                />
              </div>
            </div>
          </div>

          <div
            class="
              relative
              bg-indigo-600
              lg:col-start-3
              lg:row-start-1
              lg:col-span-10
              lg:rounded-3xl
              lg:grid
              lg:grid-cols-10
              lg:items-center
            "
          >
            <div
              class="
                hidden
                absolute
                inset-0
                overflow-hidden
                rounded-3xl
                lg:block
              "
              aria-hidden="true"
            >
              <svg
                class="
                  absolute
                  bottom-full
                  left-full
                  transform
                  translate-y-1/3
                  -translate-x-2/3
                  xl:bottom-auto xl:top-0 xl:translate-y-0
                "
                width="404"
                height="384"
                fill="none"
                viewBox="0 0 404 384"
                aria-hidden="true"
              >
                <defs>
                  <pattern
                    id="64e643ad-2176-4f86-b3d7-f2c5da3b6a6d"
                    x="0"
                    y="0"
                    width="20"
                    height="20"
                    patternUnits="userSpaceOnUse"
                  >
                    <rect
                      x="0"
                      y="0"
                      width="4"
                      height="4"
                      class="text-indigo-500"
                      fill="currentColor"
                    />
                  </pattern>
                </defs>
                <rect
                  width="404"
                  height="384"
                  fill="url(#64e643ad-2176-4f86-b3d7-f2c5da3b6a6d)"
                />
              </svg>
              <svg
                class="
                  absolute
                  top-full
                  transform
                  -translate-y-1/3 -translate-x-1/3
                  xl:-translate-y-1/2
                "
                width="404"
                height="384"
                fill="none"
                viewBox="0 0 404 384"
                aria-hidden="true"
              >
                <defs>
                  <pattern
                    id="64e643ad-2176-4f86-b3d7-f2c5da3b6a6d"
                    x="0"
                    y="0"
                    width="20"
                    height="20"
                    patternUnits="userSpaceOnUse"
                  >
                    <rect
                      x="0"
                      y="0"
                      width="4"
                      height="4"
                      class="text-indigo-500"
                      fill="currentColor"
                    />
                  </pattern>
                </defs>
                <rect
                  width="404"
                  height="384"
                  fill="url(#64e643ad-2176-4f86-b3d7-f2c5da3b6a6d)"
                />
              </svg>
            </div>
            <div
              class="
                relative
                max-w-md
                mx-auto
                py-12
                px-4
                space-y-6
                sm:max-w-3xl sm:py-16 sm:px-6
                lg:max-w-none lg:p-0 lg:col-start-4 lg:col-span-6
              "
            >
              <h2 class="text-3xl font-extrabold text-white" id="join-heading">
                Profile picture generator
              </h2>
              <p class="text-lg text-white">
                Upload any image with your face in the input below. We will find
                your face in the picture and crop it out for you to use as your
                profile picture.
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div
      class="
        bg-white
        px-4
        py-5
        border
        rounded-lg
        border-gray-200
        sm:px-6
        w-2/3
        mx-auto
        text-center
      "
    >
      <a
        v-if="cloudinaryImage"
        target="_blank"
        :href="imageUrl"
        class="
          inline-flex
          items-center
          px-4
          py-2
          border border-transparent
          text-sm
          font-medium
          rounded-md
          text-indigo-700
          bg-indigo-100
          hover:bg-indigo-200
          focus:outline-none
          focus:ring-2
          focus:ring-offset-2
          focus:ring-indigo-500
        "
      >
        Download
      </a>
      <form v-else @submit.prevent="upload">
        <input
          class="
            my-5
            block
            w-full
            border border-gray-100
            rounded-md
            shadow-sm
            py-2
            px-3
            focus:outline-none focus:ring-indigo-500 focus:border-indigo-500
            sm:text-sm
          "
          type="file"
          name="file"
          @change="handleFile"
        />
        <p v-if="uploading" class="text-center text-gray-700">Uploading...</p>
        <button
          v-else-if="image"
          type="submit"
          class="
            block
            mx-auto
            items-center
            px-4
            py-2
            border border-transparent
            text-sm
            font-medium
            rounded-md
            shadow-sm
            text-white
            bg-indigo-600
            hover:bg-indigo-700
            focus:outline-none
            focus:ring-2
            focus:ring-offset-2
            focus:ring-indigo-500
          "
        >
          Convert
        </button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploading: false,
      image: null,
      cloudinaryImage: null,
    };
  },
  computed: {
    imageUrl() {
      return this.cloudinaryImage
        ? this.$cloudinary.image.url(this.cloudinaryImage.public_id, {
            gravity: "faces",
            height: 200,
            width: 200,
            crop: "thumb",
          })
        : "https://via.placeholder.com/200";
    },
  },
  methods: {
    async handleFile(e) {
      this.image = e.target.files[0];
    },
    async readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
    async upload() {
      this.uploading = true;
      const imageData = await this.readData(this.image);
      this.cloudinaryImage = await this.$cloudinary.upload(imageData, {
        upload_preset: "default-preset",
        folder: "nuxtjs-profile-picture-generator",
      });
      console.log(this.cloudinaryImage);
      this.uploading = false;
    },
  },
};
</script>

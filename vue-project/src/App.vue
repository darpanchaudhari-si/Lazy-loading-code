<template>
  <div>
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div v-for="(image, index) in images" :key="index" class="swiper-slide">
          <img
            :alt="image.alt"
            :data-src="image.src"
            class="swiper-lazy"
            ref="lazyImages"
          />
          <div class="swiper-lazy-preloader"></div>
        </div>
      </div>
      <!-- Swiper Pagination -->
      <div class="swiper-pagination"></div>
      <!-- Swiper Navigation -->
      <div class="swiper-button-next"></div>
      <div class="swiper-button-prev"></div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import Swiper from 'swiper/bundle';
import 'swiper/swiper-bundle.css';

export default {
  data() {
    return {
      images: ref([
        {
          src: 'https://img.freepik.com/free-photo/beautiful-scenery-summit-mount-everest-covered-with-snow-white-clouds_181624-21317.jpg?w=626&q=75&ext=jpg&234',
          alt: 'Image 1',
        },
        {
          src: 'https://img.freepik.com/free-photo/flag-india_1401-132.jpg?w=626&q=75&ext=jpg',
          alt: 'Image 2',
        },
        {
          src: 'https://img.freepik.com/free-vector/realistic-set-colorful-powder-clouds-explosions-isolated-transparent-background_1441-2628.jpg?w=626&q=75&ext=jpg',
          alt: 'Image 3',
        },
        {
          src: 'https://plus.unsplash.com/premium_photo-1666863911660-d64fc1022c12?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8anBnfGVufDB8fDB8fHww',
          alt: 'Image 4',
        },
        {
          src: 'https://images.unsplash.com/photo-1598124146163-36819847286d?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8anBnfGVufDB8fDB8fHww',
          alt: 'Image 5',
        }
      ]),
    };
  },
  mounted() {
    this.setupSwiper();
    this.observeLazyImages();
  },
  methods: {
    setupSwiper() {
      new Swiper('.swiper-container', {
        slidesPerView: 1,
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev',
        },
        pagination: {
          el: '.swiper-pagination',
          type: 'bullets',
        },
        lazy: true, // Enable lazy loading
      });
    },
    observeLazyImages() {
      const options = {
        root: null,
        rootMargin: '10px',
        threshold: 0.1,
      };

      let observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            let img = entry.target;
            if (img.dataset.src) {
              img.src = img.dataset.src;
              observer.unobserve(img);
            }
          }
        });
      }, options);

      this.$nextTick(() => {
        const lazyImages = this.$refs.lazyImages;
        lazyImages.forEach((img) => {
          observer.observe(img);
        });
      });
    },
  },
};
</script>

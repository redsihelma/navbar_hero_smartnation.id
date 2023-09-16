<template>
  <div class="hero">
    <div
      class="hero-carousel"
      ref="carousel"
      @touchstart="startSwipe"
      @touchmove="moveSwipe"
      @touchend="endSwipe"
      @mousedown="startSwipe"
      @mousemove="moveSwipe"
      @mouseup="endSwipe"
    >
      <div class="carousel-inner">
        <div
          class="hero-item"
          :class="{ active: index === currentIndex }"
          v-for="(item, index) in items"
          :key="index"
        >
          <div class="hero-overlay">
            <img :src="item.image" alt="Hero Image" class="hero-image" />
            <div class="rectangle"></div>
            <div class="cont">
              <h1 class="hero-title">
                <div class="social-media-images">
                  <!-- Menggunakan gambar media sosial -->
                  <a class="ig" href="#"
                    ><img class="ins" src="/images/ig.png" alt="Instagram"
                  /></a>
                  <a class="fb" href="#"
                    ><img class="fcb" src="/images/fb.png" alt="Facebook"
                  /></a>
                  <a class="wa" href="#"
                    ><img class="wht" src="/images/wa.png" alt="Whatsapp"
                  /></a>
                  <a class="tt" href="#"
                    ><img class="tktk" src="/images/tiktok.png" alt="Tiktok"
                  /></a>
                </div>
                {{ item.title }}
              </h1>
            </div>
          </div>
        </div>
      </div>

      <div class="hero-dots">
        <span
          v-for="(dot, dotIndex) in items"
          :key="dotIndex"
          class="dot"
          :class="{ active: dotIndex === currentIndex }"
          @click="goTo(dotIndex)"
        ></span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          title: "Asyik, Teknologi 6G Sudah Disiapkan. Apa Saja Manfaatnya?", // Tambahkan judul untuk setiap item
          image: "/images/hero1.png",
        },
        {
          title:
            "Beberapa Teknologi Penanganan Banjir di Sejumlah Negara, Apakah Indonesia Punya?",
          image: "/images/hero2.png",
        },
        {
          title:
            "Smart City: Standar Nasional Indonesia (SNI) untuk Kota Cerdas",
          image: "/images/hero3.png",
        },
        {
          title:
            "Terapkan Smart Fisheries Village (SFV) pada Industri Kelautan di Indonesia",
          image: "/images/hero4.png",
        },
      ],
      currentIndex: 0,
      touchStartX: 0,
      touchEndX: 0,
      isDragging: false,
    };
  },
  methods: {
    goTo(index) {
      this.currentIndex = index;
    },
    next() {
      if (this.currentIndex < this.items.length - 1) {
        this.currentIndex++;
      } else {
        this.currentIndex = 0;
      }
    },
    prev() {
      if (this.currentIndex > 0) {
        this.currentIndex--;
      } else {
        this.currentIndex = this.items.length - 1;
      }
    },
    startSwipe(event) {
      this.isDragging = true;
      this.touchStartX =
        event.type === "touchstart" ? event.touches[0].clientX : event.clientX;
    },

    moveSwipe(event) {
      if (!this.isDragging) return;

      this.touchEndX =
        event.type === "touchmove" ? event.touches[0].clientX : event.clientX;
    },

    endSwipe() {
      if (!this.isDragging) return;

      const deltaX = this.touchEndX - this.touchStartX;

      if (deltaX > 50) {
        // Geser ke kiri
        this.prev();
      } else if (deltaX < -50) {
        // Geser ke kanan
        this.next();
      }

      this.isDragging = false;
    },
  },
  created() {
    setInterval(this.next, 10000);

    // Memperbarui judul di tengah gambar saat berpindah
    this.$watch("currentIndex", (newIndex) => {
      // Anda dapat memilih elemen judul yang sesuai sesuai dengan indeks currentIndex
      const titleElement = document.querySelector(
        ".hero-item.active .hero-title.active"
      );
      if (titleElement) {
        titleElement.textContent = this.items[newIndex].title;
      }
    });
  },
};
</script>

<style scoped>
.cont {
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: flex-start;
  height: 289px;
}
.carousel-inner {
  position: relative;
}

.hero {
  position: relative;
  overflow: hidden;
}

.rectangle {
  padding: 0;
  height: 690px !important;
  flex-shrink: 0;
  position: relative;
  width: 100%;
  background: linear-gradient(
    90deg,
    #a60b40 17.23%,
    rgba(217, 217, 217, 0) 99.38%
  );
  /* background: radial-gradient(
    120% 150% at 0% 100%,
    #a60b40 0%,
    rgba(217, 217, 217, 0) 90%
  ); */
}

.hero-carousel {
  display: flex;
  transition: transform 0.5s ease-in-out;
  max-width: 100%;
  margin: 0 auto;
  height: 700px;
}

.hero-item {
  flex: 0 0 100%;
  width: 100%;
  position: absolute;
  opacity: 0;
  transition: opacity 0.5s;
  height: 100%;
  overflow: hidden;
}

.hero-image {
  position: absolute;
  padding: 0;
  width: 100%;
  height: 690px !important;
}

.hero-item.active {
  opacity: 1;
}

.hero-overlay {
  overflow: hidden;
  align-items: center;
  flex-direction: column;
}

.hero-title {
  font-size: 42px;
  line-height: 150%; /* 63px */
  margin-bottom: 0;
  text-align: left;
  width: 975.46px !important;
  height: 126px;
  flex-shrink: 0;
  color: var(--font-50, #f6f6f6);
  /* Font/Heading 3 Bold */
  font-family: Poppins;
  font-style: normal;
  font-weight: 700;
  position: absolute;
  top: 46%;
  margin-left: 400px;
  padding-left: 14.445%;
  transform: translate(-50%, -50%);
}

.hero-dots {
  display: flex;
  width: 1440px;
  padding: 19.2px 28.8px;
  padding-top: 99.2px;
  justify-content: center;
  align-items: center;
  text-align: center;
  position: absolute;
  bottom: 10.6%;
  left: 50%;
  gap: 9.6px;
  transform: translateX(-50%);
  z-index: 1;
}

.dot {
  display: inline-block;
  width: 9.6px;
  height: 9.6px;
  background-color: #bdbdbd;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s;
}

.dot.active {
  background-color: #d71149;
}

/* CSS untuk ikon media sosial */
.social-media-images {
  position: absolute;
  padding-left: 119.6% !important;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 55.405px;
  width: 1199.65px;
  height: 289px !important;
}

/* Ganti warna ikon sesuai kebutuhan Anda */
.social-media-images a:hover {
  color: #007bff; /* Warna saat hover */
}
.ig {
  width: 32.567px !important;
  height: 30.588px !important;
  fill: var(--font-200, #d1d1d1);
}
.ins {
  width: 32.567px !important;
  height: 30.588px !important;
  fill: var(--font-200, #d1d1d1);
  margin-bottom: 40px;
}

.fb {
  width: 16.426px !important;
  height: 29.681px !important;
  fill: var(--font-200, #d1d1d1);
}

.fcb {
  width: 16.426px !important;
  height: 29.681px !important;
  fill: var(--font-200, #d1d1d1);
  margin-bottom: 40px;
}

.wa {
  width: 31.043px !important;
  height: 31.184px !important;
}

.wht {
  width: 31.043px !important;
  height: 31.184px !important;
  margin-bottom: 40px;
}

.tt {
  width: 31.04px !important;
  height: 31.04px !important;
  fill: var(--font-200, #d1d1d1);
}

.tktk {
  width: 31.04px !important;
  height: 31.04px !important;
  fill: var(--font-200, #d1d1d1);
  margin-bottom: 40px;
}
</style>

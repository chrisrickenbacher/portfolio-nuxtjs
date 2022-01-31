<template>

  <div>


    <header class="intro">
      <div class="logo">
      <h1>Miskolczy</h1>
      <h2>Archive</h2>
      </div>
    </header>


    <main lang="en">
      <div :id="project.slug" class="project" v-for="(project, pIdx) of projects" :key="pIdx">
        <div class="title">
          <h3 class="uppercase">{{ project.title }}</h3>
          <h4 class="uppercase">{{ project.category }}</h4>
        </div>

        <article id="column">
          <nuxt-content class="content" :document="project" />
        </article>
        <div class="gallery custom-scrollbar">    
          <img v-for="(image, Idx) in project.gallery" :key="Idx" :src="require(`~/assets/images/${image}`)" loading="lazy" />
        </div>
      </div>
    </main>

    <footer>
      <div class="title">
        <a href="#"><span class="uppercase">linkedin.com/</span><wbr>in/miskolczy</a>
        <a href="#"><span class="uppercase">medium.com/</span><wbr>@miskolczy</a>
      </div>
      <address>
        <p>Impressum</p>
        <p>Konzept und Gestaltung<br>
        Schrift – Dorn</p>
        <p>Jonathan Pan Miskolczy<br>
        Leopoldstrasse 9<br>
        DE-79576 Weil am Rhein<br>
        <p>mail(at)miskolczy.net</p>
      </address>
    </footer>
    
  </div>

</template>


<script>
export default {
  //  head: {
  //   script: [
  //     { src: '/js/script.js', body: true },
  //   ]
  // },

  async asyncData({ $content, params }) {
    const projects = await $content('archive', params.slug)
    .sortBy('createdAt', 'asc')
    .fetch();
 
  return {
    projects
    }
  },

  methods: {
    variableWidthAxis() {
      var column = document.getElementById("column");
      var container = document.querySelector("body");
      const maxWidthAxis = 200;
      const minWidthAxis = 50;
      const maxColumnWidth = 600;
      const minColumnWidth = 375;
      const width = column.clientWidth;
      const percent = (width - minColumnWidth) / (maxColumnWidth - minColumnWidth);
      const scale = percent * (maxWidthAxis - minWidthAxis) + minWidthAxis;
      const newWidth = width > maxColumnWidth ? maxWidthAxis : width < minColumnWidth ? minWidthAxis : scale;
      container.style.setProperty("--width-axis", newWidth.toString());
    }
  },
  mounted: function() {
    this.variableWidthAxis
    window.addEventListener('resize',  this.variableWidthAxis);
    
  },
  unmounted() {
    window.removeEventListener('resize', this.variableWidthAxis);
  },
}
</script>

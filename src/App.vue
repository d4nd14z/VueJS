<script setup>
  import { onMounted, ref, reactive } from 'vue';
  import BlogPost from './components/BlogPost.vue';
  import PaginatePost from './components/PaginatePost.vue';
  import LoadingSpinner from './components/LoadingSpinner.vue';

  const itemsByPage = 6; //Numero de items por pagina.
  const posts = ref([]); //Se utiliza ref para reactividad en tipos primitivos
  const loading = ref(true);
  const pager = reactive({  //Se utiliza reactive para reactividad en tipos derivados (objetos)
    start: 0,
    end: itemsByPage,
    total: 0
  });

  //Utilizacion del metodo onMounted para ejecutar un script en el momento en que se realiza la carga del componente.
  //Llamado a API jsonplaceholder.typicode.com para obtener posts (utilizando Async / Await)     
  onMounted(() => {
    fetchData();
  });
  
  const fetchData = async () => {
    let url = 'https://jsonplaceholder.typicode.com/posts';
    loading.value = true; //Cargando
    try {
      const res = await fetch(url);
      posts.value = await res.json();
      pager.total = posts.value.length; 
    }
    catch(error){
      console.log(error);
    }
    finally {
      loading.value = false;
    }

    //Llamado a API jsonplaceholder.typicode.com para obtener posts (utilizando fetch. Debe eliminarse el async y el await)     
    // fetch('https://jsonplaceholder.typicode.com/posts')
    //   .then(res => res.json())
    //   .then((data) => { 
    //     posts.value = data;
    //     pager.total = posts.value.length; 
    //   })
    //   .catch((error) => {
    //     console.log(error);
    //   })
    //   .finally(() => {        
    //     loading.value = false;        
    //   });
  };

  //Metodos Handlers que son llamados por "emit" desde el componente hijo PaginatePost
  const prevHandler = () => {    
    if (pager.start >= itemsByPage) {
      pager.end = pager.start;
      pager.start -= itemsByPage;       
    }
  }

  //Metodos Handlers que son llamados por "emit" desde el componente hijo PaginatePost
  const nextHandler = () => {    
    if (pager.end < posts.value.length) {
      pager.start = pager.end;
      pager.end += itemsByPage;      
    }    
  }

</script>

<template>
  <LoadingSpinner v-if="loading"/>
  <div class="container mx-auto" v-else>    
    <h1 class="text-4xl font-bold text-black underline underline-offset-8 decoration-1 decoration-gray-300 mt-5 mb-5">VueJS 3 - https://jsonplaceholder.typicode.com/posts</h1>    
    <PaginatePost :pager="pager"                  
                  @prevHandler="prevHandler" 
                  @nextHandler="nextHandler" />
    <div class="grid grid-cols-2 gap-3">
      <BlogPost v-for="p in posts.slice(pager.start, pager.end)" :key="p.id" :id="p.id" :title="p.title" :content="p.body" :image="p.img"/>
    </div>        
  </div>
</template>

<style scoped>
</style>

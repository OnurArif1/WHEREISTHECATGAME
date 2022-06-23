<template>
  <div class="game-area">
    <!--<p> {{ answer }}</p>-->
    <h1 class="title">Poğaça <span>Nerede</span> <strong>?</strong></h1>
    <h4 class="description">Açık kartlardan birini seçtikten sonra, kapalı olan karta tıklayınız</h4>
    <div class="container"> <!--Kedilerin bulunduğu kartların yapılandırlıdğı yer-->
      <transition-group name="rotate-all" appear class="card-container">
        <app-card
          :key="card.id"
          :class="{'shadow' : selectedCard == card.id}"
          @click.native="selectedCard = card.id" 
          v-for="card in cards"
          :card="card"> 
          <!--@click.native; normmal bir html etiketi olmadığı için @click binding 'nin kullanılabilmesi için native özelliği verilir-->
          <!--@click.native="selectedCard = card.id"; burada selectedCard verisi ile hangi karta tıkladığımızı kontrol edebildik-->
          <!--:class="{'shadow' : selectedCard == card.id}";shadow isimli class'ı selectedCard hangi card.ıd 'de ise gel shadow'u ekle-->
          <!--:key="card.id";transition-group kullandığımız zaman buna animasyon verbilmemiiz için içerisindeki her bir elementin kendisini tasfir edebileceği uniq bir key ihtiyaç var o yüzden bunu kullandık-->
        </app-card>
      </transition-group>
    </div>
    <div class="container"> <!--Kedilerin bulunuğu kartlarınn aşağısındaki kartın yapılandırıldığı yer-->
      <transition name="rotate" mode="out-in">
        <component
          @click.native="showCard(answer)"
          :card="answer"
          :is="activeCard">
          <!--:is="activeCard"; aşağıda oluşturduğun component'i burada tanımladık. Yani app-default-card ile bunun farkı yok.-->
          <!--@click.native="showCard(answer)"; click olduğunda cevap verilen kardı göster-->
          <!-- :card="answer"; bununla beraber answer'ı card.vue dosyasına gönderecez -->
        </component>
      </transition>
    </div>
  </div>
</template>
<script>
  import Card from "./Card"; //card.vue dosyasını burada kullanmak için import ettik
  import DefaultCard from "./DefaultCard"; //default.vue dosyasını burada kullanmak için import ettik

  export default {
    components: {
      appCard: Card,
      appDefaultCard: DefaultCard
    },
    data() {
      return {
        selectedCard: null, 
        answer : {},
        activeCard : "app-default-card", //burada yukarda oluşturduğunuz componenti burada activeCard 'a atadık
        cards: [ //burada bütün kartlları tanımladık
          {id: 1, component: "app-card", image: "/src/assets/card-1.jpg"},
          {id: 2, component: "app-card", image: "/src/assets/card-2.jpg"},
          {id: 3, component: "app-card", image: "/src/assets/card-3.jpg"},
          {id: 4, component: "app-card", image: "/src/assets/card-4.jpg"},
          {id: 5, component: "app-card", image: "/src/assets/card-5.jpg"},
        ]
      }
    },
    created(){
      let answer = Math.ceil(Math.random() * this.cards.length); //burada 1-5 arasında random bir şekilde card üretiyor
      this.answer = this.cards[answer - 1]; //yukraıda tanıomladığın answeri çağır ve ona her seçtiğinin 1 eksiğini ata dedik
      console.log(answer);
    },
    methods : {
      showCard(answer){
        if(this.selectedCard == null){
          alert("İlk Olarak Bir Kart Seçiniz!");
        } else {
          this.activeCard = answer.component; //activeCard seç ve answer içindeki componenti ata. Çünkü; answer'ın içindeki component'te app-card isimli component bulunur
          setTimeout(() => {
            if(answer.id == this.selectedCard){
              this.$emit("activeComponentEvent", "app-celebrate");
            } else {
              this.$emit("activeComponentEvent", "app-failure");
            }
          },1000)
        }
      }
    }
  }

</script>
<style scoped>/*scoped:yazılan kodların sadece bu componente özel olmsını sağlar*/
  .title {
    text-align: center;
    color: rosybrown;
  }
  .title span {
    color: mediumpurple;
  }
  .title strong {
    color: darkred;
  }
  .description {
    color: grey;
    text-align: center;
  }
  .container, .card-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 50px;
  }
  .shadow{ /*oluşturduğumuz card elemenetlerine gölgelendirme verdik*/
    box-shadow: 0px 5px 48px #30969f!important; /* important; miras alma yoluyla CSS'e gelen özellikleri devre dışı bırakarak important kullandığınız satırdaki özelliği uyguladık anlamına geldi*/
    transition: box-shadow .5s;
  }

  /*************** Açık Kartların Animasyonlari için Gerekli Olan Transition Class tanimlari *************/
  .rotate-all-enter{}
  .rotate-all-enter-active{
    animation : rotate-all ease-in-out 2s forwards;
  }
  .rotate-all-leave{}
  .rotate-all-leave-active{}
  @keyframes rotate-all {
    from{
      transform: rotateY(0);
    }
    to{
      transform: rotateY(1080deg);
    }
  }
  /*************** Kapalı Kartın Animasyonlari için Gerekli Olan Transition Class tanimlari *************/
  .rotate-enter{}
  .rotate-enter-active{
    animation: rotate-in .5s ease-in-out forwards;
  }
  .rotate-leave{}
  .rotate-leave-active{
    animation: rotate-out .5s ease-in-out forwards;
  }
  @keyframes rotate-in {
    from{
      transform: rotateY(90deg);
    }
    to{
      transform: rotateY(0deg);
    }

  }
  @keyframes rotate-out {
    from{
      transform: rotateY(0deg);
    }
    to{
      transform: rotateY(90deg);
    }

  }
</style>


















)<template>
<div>
    <section>

        <h2 id="porfolio">{{titre}}</h2>
        

        <ul><div v-for="menu in this.LeMenu[this.ind].Type" :key="menu.numero" class="image">
           
           
            <li>
           
            <div class="containerImage">
                <img :src="require(`@/assets/${menu.num}/${menu.cheminImage}.${menu.extension}`)" height="100px"
                    width="150px" class="MenuRepas" :alt="menu.altTexte">
                    <div class="overlay">
                        <div class="text">
                        <p>{{menu.description}}</p>

                        </div>
                    </div>
                
            </div>
        </li>


        </div></ul>
        
    </section>
</div>
</template>

<script>

export default {
  name: 'carte',
  props: {
    msg: String
  },
    data(){
        return{
           titre:'Menu',
            icon:"1",
            monIndex : 0,
            
            LeMenu : [{
                nom:"Italien",
                Type : [
                    {
                        num: "1",
                        numero:1,
                        altTexte:"Salade",
                        cheminImage:"1",
                        extension:"jpeg",
                        description:"Entrée : Salade made in Italy",
                    },
                    {
                        num: "1",
                        numero:2,
                        altTexte:"Pasta",
                        cheminImage:"2",
                        extension:"jpg",
                        description:"Plat : Pate faites maison à l'Italienne",
                    },{
                        num: "1",
                        numero:3,
                        altTexte:"Tiramisu",
                        cheminImage:"3",
                        extension:"jpg",
                        description:"Déssert : Tiramisu Milanais",
                    }
                ]},{
                    nom:"Libanais",
                Type : [
                    {
                        num: "2",
                        numero:1,
                        altTexte:"Salade",
                        cheminImage:"1",
                        extension:"jpg",
                        description:"Entrée : Taboulé à la libanaise",
                    },
                    {
                        num: "2",
                        numero:2,
                        altTexte:"Plat",
                        cheminImage:"2",
                        extension:"jpeg",
                        description:"Plat : Tapas à la Libanaise ",
                    },{
                        numero:3,
                        num: "2",
                        altTexte:"Déssert",
                        cheminImage:"3",
                        extension:"jpg",
                        description:"Déssert : Douceur d'Orient",
                    }
                ],}
            ]
        }
    },
    created () {
    const index = Math.floor(Math.random() * this.LeMenu.length);
    this.menuChef = this.LeMenu[index] ;
    this.ind=index;
    this.indexString = String(index +1);
  }
}
</script>
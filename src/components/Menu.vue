)<template>
<div>
    <section>
        

        <div class="containerImage">
                <h1 src="" height="100px"
                    width="150px" class="MenuT" ></h1>

                    <div class="text">
                        <h2>{{menuChef}}  </h2>

         </div>
                
            </div>
            
    </section>
</div>
</template>

<script>

export default {
  name: 'Menu',
  props: {
    msg: String
  },
    data(){
        return{
            titre:'Menu',
            TypeMenu : [{type:"Midi", numero:0, prix:"19€"},{type:"Gastronomique", numero:1, prix:"38€"}],
        }
    },
    created () {
    const index = Math.floor(Math.random() * this.TypeMenu.length);
    this.menuChef =  this.TypeMenu[index].type + " Prix " + this.TypeMenu[index].prix;
  }
}
</script>
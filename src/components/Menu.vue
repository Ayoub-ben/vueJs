)<template>
<div>
    <section>
        

        <div class="containerImage">
                <h1 src="" height="100px"
                    width="150px" class="MenuT" ></h1>

                    <div class="text">
                        <p>{{menuChef}}  </p>

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
            TypeMenu : [{type:"Menu Midi", numero:0, prix:"19€"},{type:"Menu Gastronomique", numero:1, prix:"38€"}],
        }
    },
    created () {
    const index = Math.floor(Math.random() * this.TypeMenu.length);
    this.menuChef =  this.TypeMenu[index].type ;
  }
}
</script>


<style>
#containerImage{
  
}
</style>
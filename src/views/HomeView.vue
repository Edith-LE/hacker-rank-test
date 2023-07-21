<script >
  import CardItem from "../components/CardItem.vue"

  export default{
    components:{
      CardItem
    },
    data(){
      return{
        dataInfo: null,
      }
    },
    mounted(){
      this.getCallInfo()
    },
    methods:{
      getCallInfo(){
        return fetch("/helpers/callHistory.json")
        .then(res => res.json())
        .then(data => {
          const reduced = data.reduce((acc, cur) => {
            const existing = acc.find((el) => el.firstName == cur.firstName)
            if(existing){
              existing.qty ++
            }else{
              acc.push({
                ...cur,
                qty: 1 
              })
              
            }
            return acc
          }, [])
          const sorted = reduced.sort((a,b) => {
            return b.qty - a.qty
          })
          
          const maped = sorted.map((item) => (
            {
              name: item.firstName, 
              lastname: item.lastName,
              qty: item.qty,
              date: new Date(item.called)
            }
          ))
          this.dataInfo = maped
          return this.dataInfo
        })
      },
      getRelativeDate(date) {
        const now = new Date();
        const diffInMilliseconds = now - date;

        const seconds = Math.floor(diffInMilliseconds / 1000);
        const minutes = Math.floor(seconds / 60);
        const hours = Math.floor(minutes / 60);
        const days = Math.floor(hours / 24);
        const months = Math.floor(days / 30);
        const years = Math.floor(months / 12);

        const rtf = new Intl.RelativeTimeFormat('en', { numeric: 'auto' });
        if (years !== 0) {
          return rtf.format(years, 'year');
        } else if (months !== 0) {
          return rtf.format(months, 'month');
        } else if (days !== 0) {
          return rtf.format(days, 'day');
        } else if (hours !== 0) {
          return rtf.format(hours, 'hour');
        } else if (minutes !== 0) {
          return rtf.format(minutes, 'minute');
        } else {
          return 'just now';
        }
        
      }
    }  
  }
</script>

<template>
  <main class="contacts">
    <div class="contacts__header">
      <p>
        My favorite contacts
      </p>

    </div>
    <div
    class="contacts__card-list" 
    v-for="(item, idx) in dataInfo">
    
    <card-item 
        :data="item"
      />

    </div>
  </main>
</template>

<style lang="scss">
.contacts{
  width: 500px;
  &__header{
    background-color: violet;
    color: white;
    font-size: 18px;
    font-weight: bold;
    text-align: center;
  }
}

</style>

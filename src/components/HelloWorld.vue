<template>
  <div class="hello">
    {{messageToLower}}
    {{price}}
    {{details}}
    {{cost}}
    <!-- {{ calculateSaleTax(shirtPrice)}} -->
    {{addCurrency(calculateSaleTax(shirtPrice))}}
    <p>The shirt costs{{shirtCost}}</p>
    <p>The hat costs {{hatCost}}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
data(){
  return {
  message: 'Hello VUE' ,
  price: 25,
  details: ['one', 'two', 'three'] ,
  currency: '$',
  salesTax: 16,
  hatPrice: 10,
  shirtPrice: 25
  }
  
},
computed:{
  messageToLower(){
    return this.message.toLowerCase();
  },
  cost(){
    //work out the price of the item including saletax
    let itemCost = parseFloat(Math.round((this.salesTax / 100) *this.price) + this.price).toFixed(2);
    // Add text before displaying the currency and amount
    let outPut = 'This item costs' +this.currency + itemCost;
    outPut += '(' +this.currency + this.price+ 'excluding salesTax)';
    return outPut;
  },
  shirtCost(){
    return this.addCurrency(this.calculateSaleTax(this.shirtPrice))
  },
  hatCost(){
    return this.addCurrency(this.calculateSaleTax(this.hatPrice))
  }
},
methods:{
  calculateSaleTax(price){
    //work out the price of the item including sales tax
    return parseFloat(Math.round((this.salesTax / 100) * price) +price).toFixed(2);
  },
  addCurrency(price){
    return this.currency + price;
  }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

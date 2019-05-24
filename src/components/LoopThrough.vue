<template>
<div>
<form>
<label for="fiterField">
Field:
<select id="filterField" v-model="filterField">
<option value="">Disable filters</option>
<option value="isActive">Active user</option>
<option value="name">Name</option>
<option value="email">Email</option>
<option value="balance">Balance</option>
<option value="registered">Date registered</option>
</select>
</label>
<label for="filterQuery" v-show="this.filterField &&
!isActiveFilterSelected()">
Query:
<input type="text" id="filterQuery" v-model="filterQuery">
</label>
<span v-show="isActiveFilterSelected()">
Active:
<label for="userStateActive">
Yes:
<input type="radio" v-bind:value="true" id="userStateActive" vmodel="
filterUserState">
</label>
<label for="userStateInactive">
No:
<input type="radio" v-bind:value="false" id="userStateInactive"
v-model="filterUserState">
</label>
</span>
</form>
<table>
<tr v-for="person in people" :key='person.index' v-show="filterRow(person)">
<td>{{ person.name }}</td>
<td>
<a v-bind:href="'mailto:' + person.email">{{ person.email }}
</a>
</td>
<td v-bind:class="balanceClass(person)">
{{ formatBalance(person.balance) }}
</td>
<td>{{ formatDate(person.registered) }}</td>
<td v-bind:class="activeClass(person)">
{{ activeStatus(person) }}
</td>
</tr>
</table>
    
</div>
</template>
<script>
export default {
   data(){
       return {
           people:[
  {
    "index": 0,
    "guid": "8b7010f0-9411-4099-8eb0-f5c7b385b56c",
    "isActive": true,
    "balance": 2194.57,
    "name": "Isabella Fisher",
    "email": "isabellafisher@splinx.com",
    "registered": "2017-04-24T01:35:05"
  },
  {
    "index": 1,
    "guid": "f0d999df-8416-4849-b90b-1417eed0db26",
    "isActive": false,
    "balance": 2941.59,
    "name": "Lessie Jenkins",
    "email": "lessiejenkins@splinx.com",
    "registered": "2014-05-11T04:52:29"
  },
  {
    "index": 2,
    "guid": "0797ac04-872d-470b-ad1d-6a160cb76d4f",
    "isActive": true,
    "balance": 3996.3,
    "name": "Ila Roy",
    "email": "ilaroy@splinx.com",
    "registered": "2018-04-07T10:49:18"
  },
  {
    "index": 3,
    "guid": "efa265b6-50b7-48d0-8398-06944cb25cc5",
    "isActive": false,
    "balance": 3342.14,
    "name": "Alberta Knowles",
    "email": "albertaknowles@splinx.com",
    "registered": "2017-01-29T01:02:12"
  },
  {
    "index": 4,
    "guid": "e9273041-a40f-41ec-8fb6-1a62d371029e",
    "isActive": false,
    "balance": 1435.05,
    "name": "Massey Riley",
    "email": "masseyriley@splinx.com",
    "registered": "2017-04-26T08:34:36"
  }
],
currency: '$',
filterField: '',
filterQuery: '',
filterUserState: ''
       }
   },
   methods:{
activeStatus(person) {
return (person.isActive) ? 'Active' : 'Inactive';
},
activeClass(person) {
return person.isActive ? 'active' : 'inactive';
},
balanceClass(person) {
let balanceLevel = 'success';
if(person.balance < 2000) {
balanceLevel = 'error';
} else if (person.balance < 3000) {
balanceLevel = 'warning';
}
let increasing = false,
balance = person.balance / 1000;
if(Math.round(balance) == Math.ceil(balance)) {
increasing = 'increasing';
}
return [balanceLevel, increasing];
},
formatBalance(balance) {
return this.currency + balance.toFixed(2);
},
formatDate(date) {
let registered = new Date(date);
return registered.toLocaleString('en-US');
},
filterRow(person) {
let result = true;
if(this.filterField) {
if(this.filterField === 'isActive') {
result = (typeof this.filterUserState === 'boolean') ?
(this.filterUserState === person.isActive) : true;
} else {
    let query = this.filterQuery,
field = person[this.filterField];
if(typeof field === 'number') {
query.replace(this.currency, '');
try {
result = eval(field + query);
} catch(e) {

}
} else {
field = field.toLowerCase();
result = field.includes(query.toLowerCase());
}
}
}
return result;
},
isActiveFilterSelected() {
return (this.filterField === 'isActive');
}
} 
}
</script>


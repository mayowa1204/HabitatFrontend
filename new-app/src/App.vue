<template>

  <img alt="Vue logo" src="./assets/logo.png">

</template>

<script>
import { jsonp } from 'vue-jsonp'
import {computed} from 'vue'
import {aunctionDataStore} from './utils/aunctionDataStore'

export default {
  name: 'App',
  setup(){
     const metadata = computed({
       get() {
        return aunctionDataStore.state.aunctionMetadata;
       },
        set(newValue) {
            aunctionDataStore.state.aunctionMetadata = newValue;
          }
    })
    
    async function getAnctionData() {
         const test = await jsonp(
        'https://data.nationalgrideso.com/api/3/action/datastore_search?resource_id=6fd8e042-be27-4c67-ad59-5acdd2a7b0fd',
      )
      const records = []
      const metadata = {
         help:test.help,
          resource_id:test.result.resource_id,
          total: test.result.total,
          include_total:test.result.include_total,
          links:{
              start:test.result._links.start,
              next:test.result._links.next,
            },
            records:records

      }
      test.result.records.forEach(record => {
            // const result = {
            //     id:record['_id'],
            //     marketName:record['MarketName'],
            //     biddingLevelName: record['BiddingLevelName'],
            //     memberName:record['MemberName'],
            //     orderID: record['OrderID'],
            //     portfolio:record['Portfolio'],
            //     orderEntryTime:record['OrderEntryTime'],
            //     orderEntryUser:record['OrderEntryUser'],
            //     settlementCurrency:record['SettlementCurrency'],
            //     clearingPrice:record['ClearingPrice'],
            //     price:record['Price'],
            //     mar:record['MAR'],
            //     status:record['Status'],
            //     blockCode:record['BlockCode'],
            //     blockCodePRM:record['BlockCodePRM'],
            //     paradoxically:record['Paradoxically'],
            //     orderPeriodID:record['OrderPeriodID'],
            //     tradeID:record['TradeID'],
            //     efa:record[ 'EFA'],
            //     deliveryStart:record['DeliveryStart'],
            //     deliveryEnd:record['DeliveryEnd'],
            //     executedVolume:record[ 'ExecutedVolume'],
            //     volume:record[ 'Volume'],
            //     invalid:record[ 'Invalid'],
            //     bidValidity:record['Bid Validity'],
            // }
            records.push(record)
            // console.log(result)
      })

  
      if(!aunctionDataStore.state.aunctionMetadata[0] ){
        console.log(aunctionDataStore.state.aunctionMetadata[metadata] )
       aunctionDataStore.state.aunctionMetadata.push(metadata)
      } 
       console.log(aunctionDataStore.state)
    
      
 
      
    }
    getAnctionData()
    


    return {metadata}

    
 
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

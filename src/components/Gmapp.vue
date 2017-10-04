<template>
  <div class="custom-gmapp row">

    <div class="col-md-8">
      <routeInfo v-if="haveinfo" :distance="distance" :duration="duration"></routeInfo>
      <Mapframe></Mapframe>
    </div>

    <div class="col-md-4">
    <form v-on:submit.prevent="createGMRoute" method="post">
      <div class="form-group">
        <input id="start" type="text" v-model="route.start" class="form-control" placeholder="From">
      </div>
      <div class="form-group">
        <input id="end" type="text" v-model="route.end" class="form-control" placeholder="To">
      </div>
      <div class="form-group">
        <button id="ok" type="submit" class="btn btn-primary">Ok</button>
      </div>
    </form>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>No.</th>
          <th>From(A)</th>
          <th>To(B)</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <gmappRoutes v-for="(route, index) in routes" v-bind:route="route" @showRoute="showRoute(route)" @delRoute="removeRoute(index)" ></gmappRoutes>
      </tbody>
    </table>
    </div>
  </div>
</template>

<script>

import gmappRoutes from './gmappRoutes';
import Mapframe from './Mapframe';
import routeInfo from './routeInfo';

export default {
  name: 'Gmapp',
  components: { gmappRoutes, Mapframe, routeInfo },
  data () {
    return {
      routes: [],
      route: {
        start: '',
        end: ''
      },
      distance: '',
      duration: '',
      haveinfo: false
    }
  },
  created(){
    this.getRoutes();
  },
  mounted(){
    this.showRoute();
  },
  methods:{
    createGMRoute(){
      var id = this.routes.length+1;
      var route = this.route;
      var routes = this.routes;

      //If user fields are not empty
      if((route.start.length != 0) && (route.end.length != 0)){
        routes.push({id: id, start: route.start, end: route.end});

        //Save in local storage
        var data = JSON.parse(localStorage['vuedata']);
        data.push({id: id, start: route.start, end: route.end});
        localStorage['vuedata'] = JSON.stringify(data);
        route.start = '';
        route.end = '';
      }
      else{
        this.$notify({
          group: 'foo',
          type: 'error',
          title: 'Input ERROR',
          text: 'Both fields are required!'
        });
      }

    },
    removeRoute(index){
      var routes = this.routes;
      routes.splice(index,1);

      //Remove from local storage
      var data = JSON.parse(localStorage['vuedata']);
      data.splice(index,1);
      localStorage['vuedata'] = JSON.stringify(data);

    },
    getRoutes(){
      var data = JSON.parse(localStorage['vuedata']);
      

      for(var i=0; i<data.length; i++){
        this.routes.push(data[i]);
      }
    },
    showRoute(route){
        var directionsService = new google.maps.DirectionsService;
        var directionsDisplay = new google.maps.DirectionsRenderer;
        var map = new google.maps.Map(document.getElementById('map'), {
          zoom: 7,
          center: {lat: 44.01787560939019, lng: 20.9011644}
        });
        directionsDisplay.setMap(map);
      
        var startPoint = route.start;
        var endPoint = route.end;
        var app = this;

        calculateAndDisplayRoute(directionsService, directionsDisplay, startPoint, endPoint);

      function calculateAndDisplayRoute(directionsService, directionsDisplay, a, b) {
        directionsService.route({
          origin: a,
          destination: b,
          travelMode: 'DRIVING'
        }, function(response, status) {
          if (status === 'OK') {
            directionsDisplay.setDirections(response);

            var rrl = response.routes[0].legs[0];
            var distance = rrl.distance.text;
            var duration = rrl.duration.text;
            var enDuration = duration.replace(/сата\(и\)/gm,"hours").replace(/сат/gm,"hour").replace(/мин/gm,"min/s");
            app.distance = distance;
            app.duration = enDuration;
            app.haveinfo = true;

          } else {
            window.alert('Directions request failed due to ' + status);
          }
        });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#ok{
  width: 100%;
}

table.table.table-striped th {
    text-align: center;
}

</style>

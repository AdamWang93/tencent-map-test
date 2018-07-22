<template>
  <div id="map">
  </div>
</template>

<script>
export default {
  name: "tencent-map",
  mounted: function() {
    this.$nextTick(function() {
      this.oGeolocation = new BMap.Geolocation();
      //   var oGeolocation = new qq.maps.Geolocation();
      //watch movement.
      //   oGeolocation.watchPosition(this.watchMovement);
      this.getCurrentLocation();
    });
  },
  methods: {
    getCurrentLocation() {
      var that = this;
      this.oGeolocation.getCurrentPosition(function(r) {
        if (this.getStatus() == BMAP_STATUS_SUCCESS) {
          that.convertLatlng(r.point);
        } else {
          alert("failed" + this.getStatus());
        }
      });
    },

    convertLatlng(oPoint) {
      var that = this;
      //change the rule from Baidu to Tencent
      qq.maps.convertor.translate(
        new qq.maps.LatLng(oPoint.lat, oPoint.lng),
        3,
        function(res) {
          var oCenter = new qq.maps.LatLng(res[0].lat, res[0].lng);
          that.oCenter = oCenter;
          if (that.oTencentMap) {
            that.oTencentMap.setCenter(oCenter);
          } else {
            that.oTencentMap = new qq.maps.Map(document.getElementById("map"), {
              center: oCenter,
              zoom: 15
            });
            var oUserLocationMarker = new qq.maps.Marker({
              position: oCenter,
              animation: qq.maps.MarkerAnimation.DROP,
              map: that.oTencentMap
            });
            that.addCustomController();
            that.getHospitalsNearby();
          }
        }
      );
    },

    watchMovement() {
      //When user move, this function will be called.
    },

    addCustomController() {
      var oBackToMyLocationBtn = document.createElement("button");
      oBackToMyLocationBtn.innerHTML = "Back to My Location";
      oBackToMyLocationBtn.style.position = "relative";
      oBackToMyLocationBtn.style.marginBottom = "2rem";
      oBackToMyLocationBtn.style.marginRight = "1rem";
      oBackToMyLocationBtn.style.height = "3rem";
      oBackToMyLocationBtn.onclick = this.backToMyLocation;
      this.oTencentMap.controls[qq.maps.ControlPosition.BOTTOM_RIGHT].push(
        oBackToMyLocationBtn
      );
    },

    backToMyLocation() {
      this.oTencentMap.setCenter(this.oCenter);
      //   this.getCurrentLocation();
    },

    getHospitalsNearby() {
      //If you have more marker, just add loop here.
      for (var i = 1; i < 3; i++) {
        var oIconFeatures = {
          size: new qq.maps.Size(24, 24)
        };
        var oCustomizeMarker = new qq.maps.MarkerImage(
          "https://www.easyicon.net/download/png/30292/48/"
        );
        var oNewMarker = new qq.maps.Marker({
          icon: oCustomizeMarker,
          map: this.oTencentMap,
          //   position: new qq.maps.LatLng(
          //     this.oCenter.lat + i,
          //     this.oCenter.lng + i
          //   )
          position: this.oCenter
        });
      }
    }
  }
};
</script>

<style>
#map {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 70%;
}
</style>

var app = angular.module('tutorialWebApp', [
  'ngRoute'
]);

/**
 * Configure the Routes
 */
app.config(['$routeProvider', function ($routeProvider) {
    $routeProvider
    // Home
        .when("/", {
            templateUrl: "partials/home.html",
            controller: "PageCtrl"
        })
       
        // else 404
        .otherwise("/404", {
            templateUrl: "partials/404.html",
            controller: "PageCtrl"
        });
}]);

/**
 * Controls the Blog
 */
app.controller('BlogCtrl', function ( /* $scope, $location, $http */ ) {
    console.log("Blog Controller reporting for duty.");
});

/**
 * Controllers of Pages
 */



app.controller('PageCtrl', ['$scope', 'PageFactory', function ($scope, PageFactory,$q) {
    console.log("Page Controller reporting for duty.");
    $scope.bookings = [];
    $scope.issues =[];
      var initial_name="";
    //Column sorting and Headers
         
     //OAUTH  
     PageFactory.initialize();
    $scope.connectButton = function() {
        PageFactory.connect().then(function() {
            if (PageFactory.isReady()) {   
                     console.log("Is Ready");    
                $('#connectButton').fadeOut(function(){
                    $('#ADD, #signOut').fadeIn();
                    
                });
            }
        });
    }


 var directionDisplay;
  var map;
  
 $scope.initialize = function() {
    directionsDisplay = new google.maps.DirectionsRenderer();
    var melbourne = new google.maps.LatLng(-37.813187, 144.96298);
    var myOptions = {
      zoom:12,
      mapTypeId: google.maps.MapTypeId.ROADMAP,
      center: melbourne
    }
    
    map = new google.maps.Map(document.getElementById("map_canvas"), myOptions);
    directionsDisplay.setMap(map);
  }
var directionsService = new google.maps.DirectionsService();
 
$scope.calcRoute = function() {
  var start = document.getElementById("start").value;
  var end = document.getElementById("end").value;
  
  var request = {
    origin:start,
    destination:end,
    travelMode: google.maps.DirectionsTravelMode.DRIVING
  };
  
  directionsService.route(request, function(response, status) {
    if (status == google.maps.DirectionsStatus.OK) {
      directionsDisplay.setDirections(response);
    }
  });
}
    //Get The Issues here 
    $scope.data = PageFactory.getIssuesDetails().then(function (response) {
        $scope.data = response;

        // Go through data to get usefull format of issues
        for (var i = 0; i <= $scope.data.length; i++) {
            var assignedTo;
            var Category = "Unknown";
            var Client = "Not assigned";
            var Priority = "Med";

            //logic for assigned check if null 

            if ($scope.data[i].asignee == null || $scope.data[i].asignee ==undefined) {
                assignedTo = "Not Assigned";
            } else {
                assignedTo = $scope.data[i].asignee.login;
            }

            // Get Category,Client and Priorities from Label field 

            if ($scope.data[i].labels.length > 0)

            {

                for (var j = 0; j < $scope.data[i].labels.length; j++) {
                   var initial_name="";

                    
                    if( $scope.data[i].labels[j].name!=undefined)
                    initial_name = $scope.data[i].labels[j].name;
                    if (initial_name.charAt(0) == 'C') {
                        Client = initial_name;
                    } else if (initial_name.charAt(0) == 'P') {
                        Priority = initial_name;
                    } else {
                        Category = initial_name;
                    }

                }


            }
            
            else {
                Category = "Unknown";
                Priority = "Med";
                Client = "Not Assigned";
            }
        

            var obj = {
                client: Client,
                id: $scope.data[i].id,
                action: $scope.data[i].title,
                description: $scope.data[i].title,
                comments: $scope.data[i].body,
                GHNumber: $scope.data[i].number,
                assigned: assignedTo,
                status: $scope.data[i].state,
                category:Category,
                priority:Priority

            };


            $scope.issues.push(obj);
        }

        console.log("IT IS DONE ");
    });

    $scope.sort = function (column) {
            if ($scope.orderProp === column) {
                $scope.direction = !$scope.direction;
            } else {
                $scope.orderProp = column;
                $scope.direction = false;
            }
        }

//Insert Issue after Oauth is done

 $scope.AddIss =function(issue)

{
PageFactory.insertIssue(issue);
}

        
}]);



/**
 * Factory to communicate with API's and APP
 */

app.factory('PageFactory', function ($http, $rootScope,$q) {
    var urlGetIssues = 'https://github.com/MachineTi/spiraleye_openerp_addons_6.1/issues/';
    var urlGetIssue = 'https://api.github.com/repos/TimoSolo/spiraleye_openerp_addons_6.1/issues';
    var urlInsertIssue ='http://api.gihub.com/repos/TimoSolo/spiraleye_openerp_addons_6.1/issues';
    var authorizationResult = false;

    //var profileId = $window.sessionStorage.id;
    var profile = {
        id: 1
    };
    return {
        getIssuesDetails: function () {
            return $http({
                // NO Headers required for this api 
                //  headers   : {'Content-Type' : 'text/html'},
                url: urlGetIssue,
                method: "GET",

            }).then(function (response) {
                //  alert("yes")
                profile = response.data;
                console.log(response);
                return profile;
            });
        },
        

     insertIssue:function(issue){
         return $http({
                   url: urlInsertBooking,
        method: "POST",
        dataType: 'json',
        data: {
          title: issue.Category,
          body: issue.Description,
          assignee: colin,
          milestone: 1,
        labels: 
               [
                issue.Client,
                issue.Priority   
               ]
          }
      }).success(function(){
           console.log("Inserted Issue")
        });

	},
        getData: function () {
            var items = []
            for (var i = 0; i <= 30; i++) {
                var item = {};
                item.id = 4 + 1;
                items.push(item);

            }
            return items;
        },
       
   //OAUth application

     initialize: function() {
            //initialize OAuth.io with public key of the application
            OAuth.initialize('gY3E-BTX8wxZlw1m1WAMBOaIswk', {cache:true});
            //try to create an authorization result when the page loads, this means a returning user won't have to click button again
            authorizationResult = OAuth.create('github');
        },
        isReady: function() {
            return (authorizationResult);
        },
        connect: function() {
               console.log("GOT HERE");
            var deferred = $q.defer();
            OAuth.popup('github', {cache:true}, function(error, result) { //cache means to execute the callback if the tokens are already present
                if (!error) {
                    authorizationResult = result;
                      console.log("HERE IT IS");
                    deferred.resolve();
                      return deferred.promise;
                } else {
                    //do something if there's an error
		      alert("ERROR IN CONNECTION");
                }
            });
            
            return deferred.promise;
        },
        clearCache: function() {
            OAuth.clearCache('github');
            authorizationResult = false;
        }
      
}

});

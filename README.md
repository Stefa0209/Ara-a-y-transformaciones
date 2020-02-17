# Ara-a-y-transformaciones
<!DOCTYPE html>
   <html>
    <head>
        <title>PROYECTO ARAÑA</title>
        <style>
            html, body { margin: 0; padding: 0; overflow: hidden; }
            #info {
                position: absolute;
                padding: 10px;
                width: 100%;
                text-align: center;
                color: #FFFFFF;
            }
        </style>
    </head>
    <body>
        <div id="info">RELACIÓN PARENT-CHILD<br/>
            Usar flecha arriba y abajo para trasladar.<br/>
            Usar flechas laterales para rotar.<br/>
        </div>
        <script src="js/three.min.js"></script>
         <script>
            var escena, aspecto, camera, renderer;
             var geometria1,geometria2,geometria3;
             var cabeza,torso;
             var patad1;
			 var patad2;
			 var patad3;
             var patai1;
			 var patai2;
			 var patai3;
             var startTime = Date.now();
             
            var upArrow = false;
		    var downArrow = false;
		    var leftArrow = false;
		    var rightArrow = false;
		    var scaleUp = false;
		    var scaleDown = false;
		    var xAxis = true;
		    var yAxis = false;
		    var zAxis = false;
        var thetaSum=0;
		var positivo=false;
    init();
    
    function init(){
 //proyeccion pantalla
            escena = new THREE.Scene();
			aspecto = window.innerWidth / window.innerHeight;
			camera = new THREE.PerspectiveCamera( 45, aspecto, 0.1, 1000);
			renderer = new THREE.WebGLRenderer();
			renderer.setSize( window.innerWidth, window.innerHeight );
			document.body.appendChild( renderer.domElement );
//eventos teclado

    var onKeyDown= function(event){
        switch( event.KeyCode){
            case 38://frente
                upArrow= true;
                break;
            case 40: //  ATRÁS
				downArrow = true;
				break;
            case 83: // aumento
				scaleUp = true;
				break;
			case 87: // disminuir
				scaleDown = true;
				break;
			case 37: // rotacion sr
			    leftArrow = true;
				break;
			case 39: // rotacion csr
				rightArrow = true;
				break;
                
        }
    };
        var onKeyUp= function (event) {
            switch( event.KeyCode){
                case 38: // tras
                    upArrow= false;
                    break;
                    case 40: // TRASLADAR
						downArrow = false;
						break;
					case 37: // ROTAR CW
						leftArrow = false;
						break;
					case 39: // ROTAR CCW
						rightArrow = false;
						break;
					case 83: // ESCALA AGRANDAR
						scaleUp = false;
						break;
					case 87: // ESCALA DISMINUIR
						scaleDown = false;
						break;
                    
            }
        };
        
        
  
             document.addEventListener( 'keydown', onKeyDown, false );
			document.addEventListener( 'keyup', onKeyUp, false );
//objeto
             var size=20;
             var divisions = size;
			var origin = new THREE.Vector3( 0, 0, 0 );
			var x = new THREE.Vector3( 1, 0, 0 );
			var y = new THREE.Vector3( 0, 1, 0 );
		  	var z = new THREE.Vector3( 0, 0, 1 );
			var color1 = new THREE.Color( 0xFFFFFF );
		  	var color2 = new THREE.Color( 0x333333 );
		  	var colorR = new THREE.Color( 0xAA0000 );
		  	var colorG = new THREE.Color( 0x00AA00 );
		  	var colorB = new THREE.Color( 0x0000AA );
			var colorRd = new THREE.Color( 0xAA6666 );
		  	var colorGd = new THREE.Color( 0x66AA66 );
		  	var colorBd = new THREE.Color( 0x6666AA );
             
             var axesHelper1 = new THREE.AxesHelper( size/20 );
			var axesHelper2 = new THREE.AxesHelper( size/20 );
			var axesHelper3 = new THREE.AxesHelper( size/20 );
		  	var gridHelperXY = new THREE.GridHelper( size, divisions, color2, color2);
		  	var gridHelperXZ = new THREE.GridHelper( size, divisions, color1, color1);
		  	var gridHelperYZ = new THREE.GridHelper( size, divisions, color2, color2 );
             
             gridHelperXY.rotateOnWorldAxis ( x, THREE.Math.degToRad(90) );
            gridHelperXZ.rotateOnWorldAxis ( y, THREE.Math.degToRad(90) );
            gridHelperYZ.rotateOnWorldAxis ( z, THREE.Math.degToRad(90) );
             
//araña

             geometria1= new THREE.SphereGeometry(1.5,5,5);
             geometria3= new THREE.SphereGeometry(1,5,5);
             
            
             geometria2 = new THREE.CylinderGeometry(.5,.5,1,25);
             for ( var i = 0; i < geometria2.faces.length; i++) { 
				if( geometria2.faces[i].normal.y != 0) { 
					geometria2.faces[i].color = colorG; 
				} 
			}
var materiale = new THREE.MeshBasicMaterial( {color: 0x00ff00} );
var materialc = new THREE.MeshBasicMaterial( {color: 0xffff33} );

var material= new THREE.MeshBasicMaterial({color: color2,vertexColors: THREE.FaceColors});
             //esfera
torso= new THREE.Mesh(geometria1,materiale);
cabeza= new THREE.Mesh(geometria3,materialc)
             //cilindro
patad1= new THREE.Mesh(geometria2,material);
patad2= patad1.clone();
patad3= patad1.clone();
patai1= patad2.clone();
patai2= patai1.clone();
patai3= patai1.clone();
		

  //patas 
  
            patad1.applyMatrix( new THREE.Matrix4().makeScale(0.5,2,0.5) );
			patad2 = patad1.clone();
		    patad3 = patad1.clone();
			patad1.applyMatrix( new THREE.Matrix4().makeTranslation(1,-1,-1) );
			patad2.applyMatrix( new THREE.Matrix4().makeTranslation( 1,-1,1) );
		    patad3.applyMatrix( new THREE.Matrix4().makeTranslation(0,-1,1) );
            patai1.applyMatrix( new THREE.Matrix4().makeScale(0.5,2,0.5) );
			patai2 = patai1.clone();
		    patai3 = patai1.clone();
		    patai1.applyMatrix( new THREE.Matrix4().makeTranslation( -1,-1,-1) );
		    patai2.applyMatrix( new THREE.Matrix4().makeTranslation(-1,-1,1) );
		    patai3.applyMatrix( new THREE.Matrix4().makeTranslation(-0,-1,-1) );
			
			torso.applyMatrix( new THREE.Matrix4().makeTranslation( 0, 1, 0) );
		    cabeza.applyMatrix( new THREE.Matrix4().makeTranslation( 1.5, 0, 0) );
        
             torso.add(cabeza);
		     torso.add(patad1);
		     torso.add(patad2);
		     torso.add(patad3);
			 torso.add(patai1);
             torso.add(patai2);
		     torso.add(patai3);
        
            escena.add( gridHelperXZ );	
			escena.add( torso );
			camera.position.x = 10;
			camera.position.y = 3;	 
		  	camera.position.z = 10;			
		  	camera.lookAt( origin );
		renderer.render( escena, camera );
            
           };
  
       function animacion() {
        render();
        requestAnimationFrame( animacion );
    }
    
    function render(){
        var dtime = Date.now()-startTime;
		var tx=0, ty=0, tz=0;	
		var sc = 1;				
		var theta=0;			
		var sigma=0;			
		
	if (thetaSum>= 60*Math.PI/180)
		positivo=false;
		
	if (thetaSum<= 60*Math.PI/180)
		positivo=true;
		
	if(upArrow){
		tx=.1; ty=0; tz=0;
		
	}	
		if(downArrow) {
			tx=-0.1; ty=0; tz=0;
			if(positivo)
				theta = .1;
			else
				theta = -.1;
		}
		thetaSum+=theta;
		
		if(rightArrow)
			sigma = -.1;
		if(leftArrow)
			sigma = .1;
		
		//MATRIZ DE TRASLACIÓN
		var t = new THREE.Matrix4();
		t.set( 	1, 0, 0, tx,
				0, 1, 0, ty, 
				0, 0, 1, tz,
				0, 0, 0, 1	);
		
		torso.matrix.multiply(t); 	//APLICAR LA TRASLACIÓN A NIVEL LOCAL
		
		//ROTACIONES
		var ct1 = Math.cos(theta);
		var ct2 = Math.cos(-theta);
		var ct3 = Math.cos(theta);
		var cs = Math.cos(sigma);
		var st1 = Math.sin(theta);
		var st2 = Math.sin(-theta);
		var st3 = Math.sin(theta);
		var ss = Math.sin(sigma);
		var r = new THREE.Matrix4();
		var r1 = new THREE.Matrix4();
		var r2 = new THREE.Matrix4();
        var r3 = new THREE.Matrix4();
		//MATRIZ DE ROTACIÓN EN EJE Y
		r.set( 	   cs,  0, ss, 0,
					0,  1,  0, 0, 
				  -ss,  0, cs, 0,
					0,  0,  0, 1 );	
		//MATRICES DE ROTACIÓN EN EJE LOCAL DE PIERNAS	
		r1.set( 	1,  0,  0, 0,
					0, ct1,-st1, 0, 
					0, st1, ct1, 0,
					0,  0,  0, 1 );	
		r2.set( 	1,  0,  0, 0,
					0, ct2,-st2, 0, 
					0, st2, ct2, 0,
					0,  0,  0, 1 );	
		r3.set( 	1,  0,  0, 0,
					0, ct3,-st3, 0, 
					0, st3, ct3, 0,
					0,  0,  0, 1 );	
		
		//ROTACION EN UN EJE PARALELO
		var tempMatrix = new THREE.Matrix();
		tempMatrix.copyPosition( hips.matrix );
		torso.applyMatrix( new THREE.Matrix4().getInverse(tempMatrix) );
		torso.applyMatrix(r);
		torso.applyMatrix( tempMatrix );

		patad1.applyMatrix(r1);
		patad2.applyMatrix(r1);
		patad3.applyMatrix(r1);
		patai1.applyMatrix(r2);
		patai2.applyMatrix(r2);
		patai3.applyMatrix(r3);
		
				
        camera.lookAt( 0, 0, 0 );
        renderer.render( scene, camera );
				
        
    }    
             
       
        </script> 
    </body>
</html>

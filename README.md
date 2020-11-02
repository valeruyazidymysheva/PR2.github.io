
<!DOCTYPE html>
<html lang="en">
	<head>
		<title>three.js webgl - геометрические фигуры</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
		<link type="text/css" rel="stylesheet" href="https://threejs.org/examples/main.css">
	</head>
	<body>
		<div id="info">
			Трехмерные фигуры
		</div>

		<script type="module">

			import * as THREE from 'https://threejs.org/build/three.module.js';

			import { OrbitControls } from 'https://threejs.org/examples/jsm/controls/OrbitControls.js';

			var camera, scene, renderer;
			var controls;
			var ambientLight, light;
			var Earth, phi=-Math.PI / 2, radius = 1200;
			init();
			animate();

			function init() {

				var container = document.createElement( 'div' );
				document.body.appendChild( container );

				// CAMERA
				camera = new THREE.PerspectiveCamera( 45, window.innerWidth / window.innerHeight, 1, 100000 );
				camera.position.set( 0, 0, 2500 );

				// LIGHTS
				ambientLight = new THREE.AmbientLight( 0x333333 );	// 0.2

				light = new THREE.PointLight( 0xFFFFFF );
				light.intensity = 3;
				light.position.set( 0, 0, 0 );
				// direction is set in GUI

				// RENDERER
				renderer = new THREE.WebGLRenderer( { antialias: true } );
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( window.innerWidth, window.innerHeight );
				container.appendChild( renderer.domElement );

				// EVENTS
				window.addEventListener( 'resize', onWindowResize, false );

				// CONTROLS
				controls = new OrbitControls( camera, renderer.domElement );
				controls.addEventListener( 'change', render );
				//controls.rotateSpeed = 1; 
				controls.enableZoom = true;  
				controls.zoomSpeed = 0.5; 			
				controls.enableDamping = true;

				// scene itself
				scene = new THREE.Scene();
				//scene.background = new THREE.Color( 0x120A2A );

				scene.add( ambientLight );
				scene.add( light );
			

				// scene objects
			
				// Earth
				
				var textureLoader = new THREE.TextureLoader();
				var map = textureLoader.load( 'earth.jpg' );
				map.wrapS = map.wrapT = THREE.RepeatWrapping;
				map.anisotropy = 16;
				var material = new THREE.MeshPhongMaterial( { map: map } );
				material.shininess = 0;
				
				var geometry = new THREE.SphereGeometry( 100, 64, 64 );
				Earth = new THREE.Mesh( geometry, material );
				scene.add( Earth );	
				
				// Sun
				
				var geometry = new THREE.SphereGeometry( 200, 64, 64 );	
				var material = new THREE.MeshBasicMaterial( { color: 0xffaa00 } );				
				var Sun = new THREE.Mesh( geometry, material );
				scene.add( Sun );

				var mapC = textureLoader.load( "glow.png" );
				var sMaterial = new THREE.SpriteMaterial( { map: mapC, color: 0xffaa00 } );
				sMaterial.blending = THREE.AdditiveBlending;	
				var sprite = new THREE.Sprite( sMaterial );
				sprite.scale.set( 500, 500, 1.0 );
				Sun.add( sprite ); 

				// Sky
				
				var geometry = new THREE.BoxGeometry ( 50000, 50000, 50000 );
				var map = textureLoader.load( 'sky.jpg' );
				map.wrapS = map.wrapT = THREE.RepeatWrapping;
				map.repeat.set( 4, 4 );
				map.anisotropy = 16;
				var material = new THREE.MeshBasicMaterial( { map: map, side: THREE.BackSide } );		
				var Sky = new THREE.Mesh( geometry, material );				
				scene.add( Sky );

			}

			// EVENT HANDLERS


			function onWindowResize() {

				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();

				renderer.setSize( window.innerWidth, window.innerHeight );

			}

			//

			function animate() {

				requestAnimationFrame( animate );
				controls.update(); //
				render();

			}

			function render() {

				Earth.position.x = radius * Math.cos( phi );  
				Earth.position.z = radius * Math.sin( phi ); 
				phi = phi + 0.001;
				Earth.rotation.y = Earth.rotation.y + 0.01;
				renderer.render( scene, camera );

			}			


		</script>

	</body>
</html>

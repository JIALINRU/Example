<!DOCTYPE html>
<html lang="en">
<head>
	<title>贪吃蛇</title>
	<meta charset="UTF-8">
	<style>
		#container{
			width: 1000px;
			border:1px solid #000;
			margin: 30px auto;
		}
		#ground{
			width: 1000px;
			height: 500px;
			position: relative;
		}
		#ground .block{
			width: 20px;
			height: 20px;
			background: #3cc;
			float: left;
		}
		#ground .snack{
			background: #000;
			position: absolute;
			top: 60px;
		}
		#ground .food{
			background: #f00;
			position: absolute;
		}
		#control{
			width: 1000px;
			height: 50px;
			line-height: 50px;
		}
		h3{
			text-align: center;
			color: green;
		}
	</style>
</head>
<body>
<h3>五块钱玩儿一次</h3>
	<div id="container">
		<div id="ground"></div>
		<div id="control">
			<button id="start">开始</button>
		</div>
	</div>
	<script>
	//1.创建操场
	var oGround = document.getElementById('ground');
	for(var i=0; i<1250; i++){
		var oDiv = document.createElement('div');
		oDiv.className = 'block';
		oGround.appendChild(oDiv);
	}
	//2.创建小蛇
	var aSnackBody = [];
	for(var i=0; i<3; i++){
		var oDiv = document.createElement('div');
		oDiv.className = 'block snack';
		oGround.appendChild(oDiv);
		oDiv.style.left = (3 - i) * 20 + 'px';
		aSnackBody.push(oDiv);
	}
	//3.创建食物
	var oFood;
	var iFoodLeft;
	var iFoodTop;
	var timer;
	function createFood(){
	do{
			var bIsFind = true; //是否找到符合条件的食物
			iFoodLeft = Math.floor(Math.random() * 50 )* 20;
			iFoodTop = Math.floor(Math.random() * 25 )* 20;
			for(var i=0; i<aSnackBody.length; i++){
				if(aSnackBody[i].offsetLeft == iFoodLeft && aSnackBody[i].offsetTop == iFoodTop){
					bIsFind = false;
					break;
				}
		}
	}while(!bIsFind);
	oFood = document.createElement('div');
	oFood.className = 'block food';
	oFood.style.left = iFoodLeft + 'px';
	oFood.style.top = iFoodTop + 'px';
	oGround.appendChild(oFood);
	}
	createFood();
	//4.小蛇驰骋
	var oStart = document.getElementById('start');
	oStart.onclick = function(){
		clearInterval(timer);
		timer = setInterval(function(){
			move();
		},300);
	};
	var direction = 'right';
	document.body.onkeydown = function(e){
		e = e || event;
		var keyCode = e.which || e.keyCode;
		switch(keyCode){
			case 38:
				if(direction != 'down'){
				direction = 'up';
				}
				break;
			case 40:
				if(direction != 'up'){
					direction = 'down';
				}
				break;
			case 37:
				if(direction != 'right'){
					direction = 'left';
				}
				break;
			case 39:
				if(direction != 'left'){
					direction = 'right';
				}
				break;
		}
	};
	var oSnackHead = aSnackBody[0];
	function move(){
		var nextPos = {
			left : oSnackHead.offsetLeft,
			top : oSnackHead.offsetTop
		};
		if(direction == 'up'){
			nextPos.top -= 20;
		}else if(direction == 'down'){
			nextPos.top += 20;
		}else if(direction == 'left'){
			nextPos.left -= 20;
		}else if(direction == 'right'){
			nextPos.left += 20;
		}
		for(var i=0; i<aSnackBody.length; i++){
			var nowPos = {
				left : aSnackBody[i].offsetLeft,
				top : aSnackBody[i].offsetTop
			};
			aSnackBody[i].style.left = nextPos.left + 'px';
			aSnackBody[i].style.top = nextPos.top + 'px';
			nextPos = nowPos;
		}
	//5.吃食物
		if(oSnackHead.offsetLeft == oFood.offsetLeft && oSnackHead.offsetTop == oFood.offsetTop){
			oFood.className = 'block snack';
			aSnackBody.push(oFood);
			createFood();
		}
		if(oSnackHead.offsetLeft > 980){
			stop();
			alert('你死了');
		}
		if(oSnackHead.offsetLeft < 0 ){
			stop();
			alert('你死了');
		}
		if(oSnackHead.offsetTop > 480){
			stop();
			alert('你死了');
		}
		if(oSnackHead.offsetTop < 0 ){
			stop();
			alert('你死了');
		}


	}
	function stop(){
		console.log('stop');
		clearInterval(timer);
	}
	
		
	</script>

</body>
</html>


















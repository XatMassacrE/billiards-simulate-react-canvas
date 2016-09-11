import React, { Component } from 'react';

const lineData = require('../curve_example.json');
console.log(lineData);
// 美式落带球
// 2810*1530*850mm
// d = 56mm
// 2600 1300
// 1300 650
// r = 14
export default class CanvasComponent extends Component {
  constructor(...args) {
    super(...args);
    this.state = {
      color: 'green',
      width: 1300,
      height: 650

    };
    this.config = {
      isMouseDown: false,
      step: 0,

      isMirror: false,
      isEdit: false,
      isCue: false,
      isObject: false,
      r: 16,
      cueBall: {
	x: 650,
	y: 400,
	color: 'white'
      },
      objectBall: {
	x: 350,
	y: 100,
	color: 'red'
      },
      pockets: [{x:0,y:0}]
    };
  }
  componentDidMount() {
    //this.updateCanvas();
    this.config.isMouseDown = false;
    this.canvas = this.refs.canvas;
    this.ctx = this.canvas.getContext('2d');
    this.ctx.fillStyle = 'rgb(0,176,46)';
    this.ctx.fillRect(0,0, this.state.width, this.state.height);
    this.ctx.restore();
    this.drawCir(this.config.cueBall);
    this.drawCir(this.config.objectBall); 
    this.drawRadial(this.config.pockets[0], this.config.objectBall);
    this.drawRadial(this.config.cueBall, this.config.objectBall); 
    let p = this.refs.angle;
    let angle = this.getAngle(this.config.pockets[0], this.config.objectBall) - this.getAngle(this.config.objectBall, this.config.cueBall);
    p.innerHTML = `母球与目标球的夹角为${angle}`;
  }
  componentWillUpdate() {
    this.clear();
    this.points.map((arr) => this.drawCir(arr));
    this.drawQua(this.points[0], this.points[1], this.points[2]);
    this.drawQua(this.points[2], this.points[3], this.points[4]);
    this.drawLine(this.points[2], this.points[1]);
    this.drawLine(this.points[2], this.points[3]);
  }
  updateCanvas() {
    const ctx = this.refs.canvas.getContext('2d');
    ctx.fillRect(0, 0, 100, 100);
  }
  clear() {
    this.refs.canvas.getContext('2d').clearRect(0, 0, this.state.width, this.state.height);
    this.ctx.fillStyle = 'rgb(0,176,46)';
    this.ctx.fillRect(0,0, this.state.width, this.state.height);
    this.ctx.restore();
    this.drawCir(this.config.cueBall);
    this.drawCir(this.config.objectBall); 
    this.drawRadial(this.config.pockets[0], this.config.objectBall);
    this.drawRadial(this.config.cueBall,this.config.objectBall); 
    let p = this.refs.angle;
    let angle = this.getAngle(this.config.pockets[0], this.config.objectBall) - this.getAngle(this.config.objectBall, this.config.cueBall);
    p.innerHTML = `母球与目标球的夹角为${angle}`;

  }
  handleEsc(e) {
    console.log(e);
    this.clear();
    this.config.isEdit = false;
  }
  handleMouseDown(e) {
    const config = this.config;
    const canvas = this.canvas;
    const ctx = this.ctx;
    let x = e.pageX - canvas.offsetLeft;
    let y = e.pageY - canvas.offsetTop;

    let point = {x: x, y: y};
    const cueBall = this.config.cueBall;
    const objectBall = this.config.objectBall;
    const distanceToCue = this.calculateDistance(point, cueBall);
    const distanceToObject = this.calculateDistance(point, objectBall);

    if (distanceToCue <= this.config.r) {
      console.log('ok');
      this.config.isCue = true;
    } else if (distanceToObject <= this.config.r) {
      console.log('ok to object');
      this.config.isObject = true;
    } else {
      console.log('not in the path');
      return;
    }
    console.log(`${x},${y}`);
    config.isMouseDown = true;
  }
  calculateDistance(point1, point2) {
    let x1 = point1.x;
    let x2 = point2.x;
    let y1 = point1.y;
    let y2 = point2.y;

    let dis = Math.hypot((x1-x2), (y1-y2));
    console.log(dis);
    return dis;
  }
  handleMouseMove(e) {
    const canvas = this.canvas;
    const ctx = this.ctx;
    const config = this.config;
    let x = e.pageX - canvas.offsetLeft;
    let y = e.pageY - canvas.offsetTop;
    let point = {};
    point.x = x;
    point.y = y;

    if (config.isCue && !this.isTwoBallsOverlap(point, config.objectBall)) {
      config.cueBall.x = x;
      config.cueBall.y = y;
    } else if (config.isObject && !this.isTwoBallsOverlap(point, config.cueBall)) {
      config.objectBall.x = x;
      config.objectBall.y = y;
    } else {
      return;
    }
    this.clear();
  }
  handleMouseUp(e) {
    this.config.isCue = false;
    this.config.isObject = false;
    this.config.isMouseDown = false;
  }
  isTwoBallsOverlap(ball1, ball2) {
    let dis = this.calculateDistance(ball1, ball2);
    if (dis >= 2 * this.config.r) {
      return false;
    } else {
      return true;
    }
  }
  drawCir(point) {
    const ctx = this.ctx;
    ctx.beginPath();
    ctx.fillStyle = point.color;
    ctx.arc(point.x, point.y, this.config.r, 0, Math.PI * 2);
    ctx.fill();
    ctx.closePath();
    ctx.restore();
  }
  getAngle(point1, point2, point3, point4) {
    let vector1X = point2.x - point1.x;
    let vector1Y = point2.y - point2.y;
    let vector2X = point4.x - point3.x;
    let vector2Y = point4.y - point3.y; 
    let radian = Math.atan(tan);
    let angle = radian * 180 / Math.PI;
    return angle;
  }
  drawRadial(point1, point2) {
    let x = point1.x - point2.x;
    let y = point1.y - point2.y;
    
    let tempX = 0;
    let tempY = 0;

    if (x > 0) {
      tempX = -1;
    } else {
      tempX = 1;
    }
    if (y > 0) {
      tempY = -1;
    } else {
      tempY = 1;
    }
    let point = {};
    let DesX = point2.x;
    let DesY = point2.y;
    let tan = Math.abs(y/x);
    console.log(tan);
    while(DesX > 0 && DesX < 1300 && DesY > 0 && DesY < 650) {
      DesX += tempX;
      DesY += tempY*tan;
    } 
    point.x = DesX;
    point.y = DesY;
    this.drawLine(point1, point);
    console.log(point);
  }
  drawLine(point1, point2) {
    const ctx = this.ctx;
    ctx.save();
    ctx.beginPath();
    ctx.strokeStyle = 'gray';
    ctx.moveTo(point1.x, point1.y);
    ctx.lineTo(point2.x, point2.y);
    ctx.stroke();
    ctx.closePath();
    ctx.restore();
  }

  drawQua(point1, controlPoint, point2) {
    const ctx = this.ctx;
    ctx.beginPath();
    ctx.moveTo(point1.x, point1.y);
    ctx.quadraticCurveTo(controlPoint.x, controlPoint.y, point2.x, point2.y);
    ctx.stroke();
    ctx.closePath();
  }
  render() {
    return (
      <div>
        <canvas id='canvas' ref='canvas' width={this.state.width} height={this.state.height} onMouseDown={this.handleMouseDown.bind(this)} onMouseMove={this.handleMouseMove.bind(this)} onMouseUp={this.handleMouseUp.bind(this)}>
          This text is displayed if your browser does not support HTML5 Canvas. Please use another browser(maybe Chrome) and try again.
        </canvas>
	<p ref='angle'>母球与目标球的夹角为</p>
      </div>
    );
  }
}

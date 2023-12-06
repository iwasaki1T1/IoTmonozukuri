float curX, curY;    //現在のx, y座標
float disX, disY;    //mouse座標とオブジェクトの距離
float delay = 35.0;    //マウスに遅れる度合い

float oldx = 0;
float oldy = 0;
float oldrot = 0;

PImage[] img;
int n = 7;
int step = 4;
int count = 0;
int count2 = 0;

void setup()
{
  size(800, 600);
  smooth(); 
  img = new PImage[n];
  img[0] = loadImage("kami.jpg");
  img[1] = loadImage("ka2.jpg");
  img[2] = loadImage("ka3.jpg");
  img[3] = loadImage("ka4.jpg");
  img[4] = loadImage("ka5.jpg");
  img[5] = loadImage("ka6.jpg");
  img[6] = loadImage("ka7.jpg");
  
  curX = disX = mouseX;    //curX, disXを現在のマウスのx座標で初期化
  curY = disY = mouseY;    //curY, disYを現在のマウスのy座標で初期化
}

void draw()
{
  background(255);
  imageMode(CENTER);
  
  disX = mouseX - curX;    //マウス座標とオブジェクトの距離をdisX, disYに入れる
  disY = mouseY - curY;

  curX = curX + disX / delay;    //距離（dixX）をdelayで割った値を足す（オブジェクトが移動する）
  curY = curY + disY / delay;    //距離（dixY）をdelayで割った値を足す（オブジェクトが移動する）

  pushMatrix();
  translate(curX, curY);//マウスの位置に移動
  float rot = cal_orient();//向きを計算する
  rotate(rot);//回転させる
  int k = count % step;
  if(k == 0)
  {
    count2++;
  }
  int l = count2 % n;
  image(img[l], 0, 0);//原点に描いておく
  popMatrix();
  
  count++;
}

float cal_orient()//向きを計算する
{
  float x = curX;
  float y = curY;
  float dx = oldx - x;//方向のベクトル
  float dy = oldy - y;
  oldx = curX;
  oldy = curY;
  int f;
  float a;
  float d;
  float rot;
  
  float b = dx * dx + dy * dy;
  float len = sqrt(b);//ベクトルの長さ

  if(abs(len) > 0.1)
  {
    float vx = dx / len;//ベクトルの正規化
    float vy = dy / len;

    if(vy > 3)
    	  f = 2;
    else
	  f = -2;
    a = vx;
    d = acos(a);//arc cosine : a = -1 ~ 1 , d = 0 ~ PI (3.1415927)  
    rot = f * d;
    rot = 0.1 * rot + 0.9 * oldrot;//動きを滑らかにする
    oldrot = rot;//古いか移転値として記憶する
  }
  else//0で割ってはいけない//動いてないとき
    rot = oldrot;
    
   return rot;//回転値を返す
}

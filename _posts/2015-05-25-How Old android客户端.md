---
layout: post
title:  "How-Old android客户端"
date:   2015-05-25 23:52:32
categories: 杂
---

* content
{:toc}


##介绍
微软的How-Old robot近期很火，是用算法来测试照片里的人年龄及性别，可点击此链接尝试：[http://www.how-old.net/](http://www.how-old.net/)。

![http://7xk2i5.com1.z0.glb.clouddn.com/wwwhow.png](http://7xk2i5.com1.z0.glb.clouddn.com/wwwhow.png)

![http://7xk2i5.com1.z0.glb.clouddn.com/wwold.png](http://7xk2i5.com1.z0.glb.clouddn.com/wwold.png)

刚好慕课网有[教程](http://www.imooc.com/learn/393)，可以学习制作android客户端。

项目使用[Face++](http://www.faceplusplus.com.cn/uc_home/)的API,流程是读取图片二维信息将其发送到Face++后台识别，获取返回的JSON数据得到识别人脸的信息，最后将其显示在图片中。难点是网络请求和框图绘制及识别信息的绘制。

我在教程的基础上进行了一些改进。

增加了相机按钮，部分代码为：


            case R.id.camera:
                File path = Environment.getExternalStorageDirectory();
                outputImage = new File(path, "testImg.jpg");
                try{
                    if(outputImage.exists()){
                        outputImage.delete();
                    }
                    outputImage.createNewFile();
                }catch (IOException e){
                    e.printStackTrace();
                }
                imageUri = Uri.fromFile(outputImage);
                Intent cameras = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                cameras.putExtra(MediaStore.EXTRA_OUTPUT,imageUri);
                startActivityForResult(cameras,CAMERA_CODE);
                break;

新建calculateInSampleSize方法，使图片显示不产生拉伸变形：

	private void resizePhoto() {
	        BitmapFactory.Options options = new BitmapFactory.Options();
	        options.inJustDecodeBounds = true;
	        Bitmap bitmap = BitmapFactory.decodeFile(currentPhotoStr, options);
	        options.inSampleSize = calculateInSampleSize(options,480, 800);
	        options.inJustDecodeBounds = false;
	        photoImg = BitmapFactory.decodeFile(currentPhotoStr,options);
	    }
	
	    public static int calculateInSampleSize(BitmapFactory.Options options, int reqWidth,
	                                            int reqHeight){
	        final int height = options.outHeight;
	        final int width = options.outWidth;
	        int inSampleSize = 1;
	        if(height > reqHeight || width > reqHeight){
	            final int heightRatio = Math.round((float) height / (float) reqHeight);
	            final int widthRatio = Math.round((float) width / (float) reqWidth);
	            inSampleSize = heightRatio < widthRatio ? heightRatio : widthRatio;
	        }
	        return inSampleSize;
	    }



新建ButtonAction类，使点击ImageButton有动作反应：

	public class ButtonAction {
	
	    /**
	     * 按下这个按钮进行的颜色过滤
	     */
	    public final static float[] BT_SELECTED=new float[] {
	            2, 0, 0, 0, 2,
	            0, 2, 0, 0, 2,
	            0, 0, 2, 0, 2,
	            0, 0, 0, 1, 0 };
	
	    /**
	     * 按钮恢复原状的颜色过滤
	     */
	    public final static float[] BT_NOT_SELECTED=new float[] {
	            1, 0, 0, 0, 0,
	            0, 1, 0, 0, 0,
	            0, 0, 1, 0, 0,
	            0, 0, 0, 1, 0 };
	
	    /**
	     * 按钮焦点改变
	     */
	    public final static View.OnFocusChangeListener buttonOnFocusChangeListener=new View.OnFocusChangeListener() {
	
	        @Override
	        public void onFocusChange(View v, boolean hasFocus) {
	            if (hasFocus) {
	                v.getBackground().setColorFilter(new ColorMatrixColorFilter(BT_SELECTED));
	                v.setBackgroundDrawable(v.getBackground());
	            }
	            else
	            {
	                v.getBackground().setColorFilter(new ColorMatrixColorFilter(BT_NOT_SELECTED));
	                v.setBackgroundDrawable(v.getBackground());
	            }
	        }
	    };
	
	    /**
	     * 按钮触碰按下效果
	     */
	    public final static View.OnTouchListener buttonOnTouchListener=new View.OnTouchListener() {
	        @Override
	        public boolean onTouch(View v, MotionEvent event) {
	            if(event.getAction() == MotionEvent.ACTION_DOWN){
	                v.getBackground().setColorFilter(new ColorMatrixColorFilter(BT_SELECTED));
	                v.setBackgroundDrawable(v.getBackground());
	            }
	            else if(event.getAction() == MotionEvent.ACTION_UP){
	                v.getBackground().setColorFilter(new ColorMatrixColorFilter(BT_NOT_SELECTED));
	                v.setBackgroundDrawable(v.getBackground());
	            }
	            return false;
	        }
	    };
	
	    /**
	     * 设置图片按钮获取焦点改变状态
	     * @param
	     */
	    public final static void setButtonFocusChanged(View inView)
	    {
	        inView.setOnTouchListener(buttonOnTouchListener);
	        inView.setOnFocusChangeListener(buttonOnFocusChangeListener);
	    }
	
	}

识别界面如下：

![http://7xk2i5.com1.z0.glb.clouddn.com/oldapp.jpg](http://7xk2i5.com1.z0.glb.clouddn.com/oldapp.jpg)

整个工程位于[How-Old.app](https://github.com/newpunch/How-Old.app),欢迎浏览。
# software_project 

정보컴퓨터공학부 201624413 권혁근

정보컴퓨터공학부 201624549 이정승

정보컴퓨터공학부 201824571 장진호

## 1. 서비스 설명
  ### * 목적
    종이에 쓰여져 있는 숫자 혹은 문자를 핸드폰에서 검색하거나 값을 가져와야 할 때 일일이 자판을 사용하여 입력해야 한다. 여기서 우리는 만약 핸드폰의 카메라를 이용하여 종이에 쓰여져 있는 숫자 혹은 문자를 인식하면 자판으로 똑같은 내용을 입력해야하는 번거러움을 줄일 수 있을 거 같아 '숫자 인식 서비스'를 만들게 되었다.
  
  ### * 문제 해결 방식
    * 데이터 셋을 만든다.
        폴더 안에 숫자 0~9까지 폴더를 생성하고 각 폴더 안에 ipad or galaxy tab을 이용하여 그린 숫자를 하나씩 저장하여 각 폴더 당 100개 총 1000개의 데이터를 저장하였다.
        
    * .npy 폴더 생성
        저장한 데이터는 컬러 데이터 이기 때문에 이것을 흑백 데이터로 변경하기 위해서 opencv에서 제공하는 BGR2Gray를 사용하엿다. 
        
        img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        
        그 후 npy파일로 변환하여 저장한다.

        np.save("./img_data.npy", xy)
        
    * 학습 모델 구성
        npy파일이 onehotcode 이기 때문에 이것을 decode해주어서 데이터 전처리를 하였다. CNN 기법을 사용하여 학습 하였고, 6개의 layer을 구성하여 훈련 모델을 생성하였다.
        
        model = tf.keras.Sequential([
        tf.keras.layers.Conv2D(24, kernel_size=(6, 6), strides=(2,2),activation='relu'),
        tf.keras.layers.BatchNormalization(scale=False),
        tf.keras.layers.Dropout(0.25),

        tf.keras.layers.Conv2D(48, kernel_size=(5, 5), strides=(2,2),activation='relu'),
        tf.keras.layers.BatchNormalization(scale=False),
        tf.keras.layers.Dropout(0.25),

        tf.keras.layers.Conv2D(64, kernel_size=(4, 4), strides=(2,2)),
        tf.keras.layers.BatchNormalization(scale=False),
        tf.keras.layers.Activation('relu'),
        tf.keras.layers.Dropout(0.25),

        tf.keras.layers.Flatten(),
        tf.keras.layers.Dense(units=200,activation='relu'),
        tf.keras.layers.BatchNormalization(scale=False),
        tf.keras.layers.Dropout(0.25),
        tf.keras.layers.Dense(units=10, activation='softmax')
        ])
        
        그후 h5파일로 만들고 tflite 파일로 변화하였다.
        
    * 앱 생성
        안드로이드 스튜디오를 사용하여 카메라를 이용하여 글자를 인식하는 프로그램을 생성한다.
        처음 앱을 만들때 카메라로 인식이 잘 안되어서 핸드폰에 그려서 인식하는 앱도 생성 하였다.
        
## 2. 사용 방법
    * 앱을 킨다.
    
    * - 사진 인식 앱
      적당한 두께로 쓰여진 숫자를 카메라로 인식 한다. 단 초록 네모영역 안에 크기를 맞춰서 버튼을 눌러야 한다.
      인식을 한 이미지는 흑백 버전으로 갤러리에 저장 되므로 확인 후 잘 되었으면 성공이다.
    ![image](https://user-images.githubusercontent.com/49871871/117303586-d32b2380-aeb7-11eb-87fc-53c80d07965a.png)


    * - 그림 인식 앱
      네모칸 안에 크기에 맞게 숫자를 그린고 detect버튼을 클릭한다. 이것도 마찬가지로 네모칸 영역에 맞춰 그려줘야 한다.
 ![image](https://user-images.githubusercontent.com/49871871/117303491-b42c9180-aeb7-11eb-9f9d-6b43ba957bb7.png)

 

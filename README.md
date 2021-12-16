## 피카소 명화 투표하기 앱

명화 선호도 투표 앱을 만들었습니다. 이 앱은, 피카소의 명화를 클릭하여 투표를 하고, 투표를 끝내고, 제일 많이 투표수를 받은 그림과 이름을 보여주고, 다른 그림들이 받은 투표 수를 보여주는 앱입니다. 프로그래밍을 공부하는 과정들이 너무나도 어려웠고, 스스로 코드를 짜는 것이 매우 어려웠습니다. 앞으로 다시 기초를 다진다는 느낌으로 하여 제가 직접 안 보고도 코드를 짤 수 있을 정도로 실력으로 키우고 싶다는 생각이 듦과 동시에 반성하는 기회가 되었습니다. 앞으로 자바 프로그래밍과 안드로이드 스튜디오를 다시 한 번 공부해보면서, 스스로 한 커뮤니티에서의 ‘게시판’을 직접 짜보고 싶다는 생각이 들었습니다. 이번 기말고사 코드를 공부하면서, 레이팅바로 표현하기, 그림의 투표수를 저장할 변수를 선언하고 초기화하는 방법, 변수 배열, 제목 배열 등 선언하기, 이벤트 리스너 생성방법, 토스트 메시지 생성 방법 등에 대하여 익힐 수 있었습니다. 2학년이 되어서는 정말 노력했구나 라는 생각이 들 수 있을만큼 공부하겠습니다.

## 메인 액티비티
```c
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        setTitle("명화 선호도 투표");

        //그림의 투표수를 저장할 변수 선언 및 초기화
        final int voteCount[] = new int[9];
        for (int i = 0; i < 9; i++)
            voteCount[i] = 0;
        // 9개의 이미지 버튼 객체 배열
        ImageView image[] = new ImageView[9];
        // 9개의 이미지 버튼 ID 배열
        Integer imageId[] = {R.id.iv1, R.id.iv2, R.id.iv3, R.id.iv4, R.id.iv5,
                R.id.iv6, R.id.iv7, R.id.iv8, R.id.iv9};
        // 9개의 그림 제목 배열
        final String imgName[] = {"게르니카", "여인의 머리", "꿈",
                "자화상", "인생", "우는 여인",
                "마졸리", "거울 앞 소녀", "기타 치는 노인"};
        //이미지뷰 개수만큼 반복
        for (int i = 0; i < imageId.length; i++) {
            final int index = i;
            //1개의 이미지뷰에 대해 클릭 리스너를 작성하는 것, 배열을 사용해 9번 반복
            image[index] = findViewById(imageId[index]);
            image[index].setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    voteCount[index]++;//이미지뷰를 클릭할 때마다 해당 그림 순번의 투표수(voteCount) 배열 값이 하나씩 증가
                    Toast.makeText(getApplicationContext(),//이미지뷰 클릭 시 그림 순번의 그림 제목, 현재 투표 수 값을 토스트 메시지로 보여줌
                            imgName[index] + ": 총 " + voteCount[index] + " 표",
                            Toast.LENGTH_SHORT).show();
                }
            });
        }
        //버튼 클릭리스너
        Button btnFinish = (Button) findViewById(R.id.btnResult);
        btnFinish.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                Intent intent = new Intent(getApplicationContext(), ResultActivity.class);
                intent.putExtra("VoteCount", voteCount);
                intent.putExtra("ImageName", imgName);
                startActivity(intent);
            }
        });
    }
}

```
## 서브액티비티
```c
public class ResultActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.result);
        setTitle("투표 결과");
        //인텐트 받고, 배열 변수를 저장
        Intent intent = getIntent();
        int[] voteResult = intent.getIntArrayExtra("VoteCount");
        String[] imageName = intent.getStringArrayExtra("ImageName");
        //이미지 파일의 아이디를 저장하는 배열
        Integer imageFileId[] = {R.drawable.guernica, R.drawable.ladyshair, R.drawable.dream,
                R.drawable.selfportrait, R.drawable.life, R.drawable.cryinglady,
                R.drawable.majolie, R.drawable.girlinfrontofmirror, R.drawable.oldguitarist};

        TextView tvTop = findViewById(R.id.tvTop);
        ImageView ivTop = findViewById(R.id.ivTop);
        int maxEntry = 0;
        for (int i = 1; i < voteResult.length; i++) {
            if (voteResult[maxEntry] < voteResult[i])
                maxEntry = i;
        }
        tvTop.setText(imageName[maxEntry]);
        ivTop.setImageResource(imageFileId[maxEntry]);
        //넘겨받은 그림 제목의 개수만큼 텍스트뷰와 레이팅바의 위젯 배열 변수 선언
        TextView tv[] = new TextView[imageName.length];
        RatingBar rbar[] = new RatingBar[imageName.length];
        //텍스트뷰와 레이팅바의 위젯 아이디 배열 선언
        Integer tvID[] = {R.id.tv1, R.id.tv2, R.id.tv3, R.id.tv4, R.id.tv5,
                R.id.tv6, R.id.tv7, R.id.tv8, R.id.tv9};
        Integer rbarID[] = {R.id.rbar1, R.id.rbar2, R.id.rbar3, R.id.rbar4,
                R.id.rbar5, R.id.rbar6, R.id.rbar7, R.id.rbar8, R.id.rbar9};


        for (int i = 0; i < voteResult.length; i++) {
            tv[i] = (TextView) findViewById(tvID[i]);
            rbar[i] = (RatingBar) findViewById(rbarID[i]);
        }//위젯 아이디 배열에 위젯 대입

        for (int i = 0; i < voteResult.length; i++) {
            tv[i].setText(imageName[i]);
            rbar[i].setRating((float) voteResult[i]);//넘겨받은 그림 제목, 레이팅바에 투표수 적용
        }
        //현재 액티비티 종료
        Button btnReturn = (Button) findViewById(R.id.btnReturn);
        btnReturn.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                finish();
            }
        });
    }
}
```

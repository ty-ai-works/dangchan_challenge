# references — 레퍼런스 영상

**영상 파일은 저장소에 올리지 않습니다.** 협회 허가 전이고, `.gitignore`로도 막아 두었습니다. 파일은 로컬에만 보관하세요.

## 사용하는 영상 (2개)

| 코드 | 이름 | 길이 | 규격 | 권장 로컬 파일명 |
|---|---|---|---|---|
| A | 쉬운 손동작 안무 | 15.15초 | 720×1276, 30fps, H.264/AAC | `ref_A_쉬운손동작_15s.mp4` |
| B | 정식 안무 | 37.81초 | 720×1276, 30fps, H.264/AAC | `ref_B_정식안무_38s.mp4` |

상세 분석은 [docs/02_레퍼런스_영상_분석.md](../docs/02_레퍼런스_영상_분석.md)를 보세요.

## 제외한 영상

기획 메모에 있던 나머지 영상(싸이커스 홍보 영상, 다른 안무 영상 등)은 개인이 촬영한 것이라 쓰지 않습니다 (결정 D5).

## 보관 위치 권장

```
/Volumes/D/AI_Projects/Claude/01_Projects/20261012_당찬동행_춤따라하기/
├── dangchan_challenge/      ← 이 저장소
└── references_local/        ← 영상 파일 (저장소 밖)
    ├── ref_A_쉬운손동작_15s.mp4
    └── ref_B_정식안무_38s.mp4
```

저장소 밖에 두면 실수로 커밋될 일이 없습니다.

## 필요할 때 쓰는 명령

**규격 확인**

```bash
ffprobe -v error -show_entries stream=codec_type,codec_name,width,height,r_frame_rate:format=duration \
  -of default=nw=1 ref_A_쉬운손동작_15s.mp4
```

**음량 확인** (A는 피크가 0.0dB라 합성 전에 정규화가 필요할 수 있음)

```bash
ffmpeg -hide_banner -i ref_A_쉬운손동작_15s.mp4 -af volumedetect -vn -f null - 2>&1 | grep -E "mean_volume|max_volume"
```

**결과 영상에 레퍼런스 음원 합성** (T6에서 방법을 확정하기 전의 참고용 예시)

```bash
ffmpeg -i result.mp4 -i ref_A_쉬운손동작_15s.mp4 \
  -map 0:v:0 -map 1:a:0 -c:v copy -c:a aac -shortest result_with_audio.mp4
```

결과 영상의 길이와 프레임이 레퍼런스와 다르면 소리가 어긋납니다. 이때는 T6에서 정한 방법을 따르세요.

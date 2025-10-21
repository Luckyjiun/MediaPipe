# Hand & Face Detector 예제 코드

이 예제는 웹캠 영상을 사용하여 **손과 얼굴을 동시에 감지**하고 화면에 표시합니다.  

```python
import cv2
from hand_detector import HandDetector
from face_detector import FaceDetector

# 웹캠 초기화
cap = cv2.VideoCapture(0)

# Detector 객체 생성
hand_detector = HandDetector()
face_detector = FaceDetector()

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # 손 감지
    frame = hand_detector.findHands(frame)
    hand_landmarks = hand_detector.findPosition(frame)

    # 얼굴 감지
    frame, faces = face_detector.findFaces(frame)
    # 각 얼굴마다 랜드마크 추출
    for face in faces:
        landmarks = face_detector.findLandmarks(frame, face)
        # 얼굴 랜드마크를 표시
        for (x, y) in landmarks:
            cv2.circle(frame, (x, y), 2, (0, 255, 0), -1)

    # 화면 출력
    cv2.imshow("Hand & Face Detection", frame)

    # ESC 키로 종료
    if cv2.waitKey(1) & 0xFF == 27:
        break

cap.release()
cv2.destroyAllWindows()
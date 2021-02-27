# cv2, PIL, torch.tensor の変換方法

## 各ライブラリ概要

- PIL: python 組み込みの画像操作ライブラリ
- cv2: opencv の python インターフェース
- torch.tensor: pytorch の訓練データに入力する形式

## 画像の高さ・幅・チャネルの順番、RGB の順番の概要

- チャネルの順番: opencv のみ BGR, 他は RGB
- h, w, c の順番:

## データ型

- PIL: PIL 独自のフォーマット (img_pil = np.array(img_pil)で変換)
- cv2: numpy.ndarray
- torch.tensor: torch.Tnsor

## 具体例

### PIL -> ndarray(cv2)

```python
from PIL import Image
import numpy as np
import cv2
img = Image.open('')
img = cv2.cvtColor(np.array(img), cv2.COLOR_RGB2BGR) # RGB -> BGR
cv2.imshow("img", img)
cv2.waitKey()
```

### ndarray(cv2) -> PIL

```python
from PIL import Image
import numpy as np
import cv2
img = cv2.imread('')
img = cv2.cvtColor(np.array(img), cv2.COLOR_BGR2RGB) # BGR -> RGB
img = Image.fromarray(img) # 数値1~255のuint8を要求
img.show(img)
```

### PIL -> torch.Tensor

```python
torchvision.transforms.ToTensor()(input)
```

### torch.Tensor -> PIL

```python
torchvision.transforms.ToPILImage()(image.float())
```

### ndarray(cv2) -> torch.Tensor

```python
torchvision.transforms.ToTensor()(input) # float32
```

---
layout: post
title: "[Rails] Polymorphic Association(다중 연광성)"
image: 
date: 2020-4-26
tag: [rails, polymorphic, 다중 연관성, modeling]
categories: []
---

얼마전 모임 서비스에 공석 알림 기능을 개발하게 되었습니다. 모임 운영을 자동화하기 위해서 고안해낸 기능인데 이 공석 알림 기능은 모임과 비디오챗에 모두 사용되었습니다. 이 과정에서 사용하게된 Rails의 Polymorphic에 대해서 이야기 하고자합니다.

### Polymorphic Association(다중 연관성)

Polymorphic Association을 번역하면 ‘다중 연관성’입니다. 말 그대로 하나의 모델이 다른 여러 모델과 관계를 맺는 것입니다. 예를들어 위에서 이야기한 ‘공석 알림’ 기능을 ‘모임’과 ‘비디오챗’에 연결하여 사용할 수 있습니다.
만약 Polymorphic을 사용하지 않는다면 아래와 같이 비슷한 유형의 정보를 담은 모델을 각자 따로 만들어야합니다.

![pre_waiting_seat](https://drive.google.com/uc?id=17hqC-OTpSJwuh7tl0n7rl-O-Ijmp7goL){: .center}{: width="60%"}

이를 하나의 '공석 알림'으로 묶기 위해서 Polymorphic Association을 사용할 수 있습니다.

![after_waiting_seat](https://drive.google.com/uc?id=102M_n0BJzkuHyiNj26Q_27SvBbomKW3R){: .center}{: width="60%"}

여기서 우리는 foreign_id만을 이용해서 공석 알림이 모임을 위한 것인지 비디오챗을 위한 것인지 알 수 없습니다. 이를 위해 하나의 정보를 더 필요로 하는데 Rails는 사용자가 Type을 저장하지 않아도 자체적으로 연결된 모델의 Type 값을 잘 넣어줍니다.
위와 같은 구조로 model을 설계하기 위해 우리는 아래와 같이 ‘모임’ 또는 ‘비디오챗’이 들어갈 수 있는 ‘Record’라는 네이밍을 통해 이들을 연결했습니다. 공석 알림 모델을 생성하기 위해서 migration 파일을 아래와 같이 설정해줄 수 있습니다.

```ruby
class CreateWaitingSeats < ActiveRecord::Migration[5.2]
  def change
    create_table :waiting_seats do |t|
      t.bigint :record_id
      t.string :record_type
      t.timestamps
    end
  end
end
```
위 코드는 references를 사용하면 더 단순하게 만들 수 있습니다.

```ruby
class CreateWaitingSeats < ActiveRecord::Migration[5.2]
  def change
    create_table :waiting_seats do |t|
      t.references :record, polymorphic: true, null: false
      t.timestamps
    end

    add_index :waiting_seats, [:record_type, :record_id], unique: true
  end
end
```
이를 기반으로 각 모델을 설정하면 다음과 같이 작성할 수 있습니다.

```ruby
class WaitingSeat < ApplicationRecord
  belongs_to :record, polymorphic: true
end

class Program < ApplicationRecord
  has_many :waiting_seats, as: :record
end

class VideoChat < ApplicationRecord
  has_many :waiting_seats, as: :record
end
```

이제 모임과 비디오챗에서 'record'를 통해 각 '공석 알림'을 검색할 수 있습니다.
<br>
ex)<br>
@video_chat.waiting_seats → 비디오챗 공석 알림,<br>
@program.waiting_seats → 모임 공석 알림,<br>
@waiting_seat.record → 모임 또는 비디오챗

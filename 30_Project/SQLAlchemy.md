- SQLAlchemy는 python에서 가장 널리 사용되는 [[ORM]] 라이브러리이자, 데이터베이스 툴킷

---
# SQLAlchemy 사용법

**1. connection 생성 (mapping 선언)**

```Python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

**2. session 생성 및 engine과 연결**

```Python
from sqlalchemy import create_engine
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine

// engine생성
engine = create_async_engine(f"postgresql+asyncpg://{self.db_user}:{self.db_password}@{self.db_host}/{self.db_name}")

// session생성 및 연결
async_session = sessionmaker(
    autocommit=False,
    autoflush=False,
    expire_on_commit=False,
    bind=engine,
    class_=AsyncSession,
)
```

**3. 생성할 모델 만들기**

- 생성한 Base를 기반으로 모델을 생성하고, `__tablename__`을 필수적으로 선언해야합니다.

```Python
class DBUser(Base):
    __tablename__ = 'users' # 필수적으로 선언 -> table의 이름

    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)
    email = Column(String, nullable=False)
    password = Column(String, nullable=False)
    inactivate = Column(Boolean, default=False)
```

- relationship생성 : relationship이 작성된 다른 쪽을 참조합니다.

```Python
from sqlalchemy.orm import relationship

#relation을 사용할 model 내부에
relationship(연결할 model, [options,,,,,])
#options 
#ex. back_populate : relation에서 연결된 부분에서 접근할시의 이름
#order by : model명.id -> id순으로 정렬
```

**4. Schema생성**

- 데이터베이스를 초기 생성할 때에는 create_all을 사용해서 생성합니다.
- 최초 한 번만 create_all을 사용합니다.
- 이미 존재하는 db에 연결할 때에는 bind를 사용합니다.

```Python
Base.metadata.create_all(engine) #1.에서 만든 base와 2.에서 만든 engine을 사용

Base.metadata.bind = engine
```

**5. CRUD사용**

```Python
# CRUD
# CREATE: table에 정보 넣기
def create():
	ed_user = User(name='ed', fullname='Ed Jones', nickname='edsnickname')
	session.add(ed_user) #값 add하고
	#commit을 하면 data가 table에 삽입됨
	session.commit()

def create_all():
	session.add_all([User(....), User(....), ......]) #한 번에 여러값을 삽입

# READ: table에 있는 정보 불러오기
def get():
	user1.session.query(User).first() #User table의 첫번째 값을 불러옴

	#filter를 사용해 원하는 값을 불러올 수 있음
	session.query(User).filter(User.name.in_(['Edwardo', 'fakeuser'])).all()

def get_all():
	session.query(User, User.name).all() #User 객체와 User 중 이름만을 select함

# UPDATE: table에 있는 정보 갱신하기
def update(item_id):
	# 해당하는 아이템 찾아오기
	item = session.query(User).filter(User.id==item_id).first()
    	# 값 변경 진행
        
        # DB에 갱신 값 저장
        session.add(item)
        session.commit()

# DELETE: table에 있는 정보 삭제하기
def delete(item_id):
	session.query(User).filter(User.id==item_id).delete()
    	session.commit

# filter_by vs. filter
# 두 함수 모두 원하는 정보를 filtering 하기 위해 사용한다.

# filter_by : 결과물 filtering
# filter : filter_by보다 좀 더 유연한 SQL 표현을 사용할 수 있다.
# -> filter 연산자를 사용할 수 있다.
# equals : ==
# not equals : !=
# like : User.name.like('%ed%')
# match : User.name.match('wendy')
```

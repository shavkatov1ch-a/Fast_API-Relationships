# SQL (Relational) Database

1. Dastur xaritasi

```shell
.
|__project name
    |__database.py
    |__models.py
    |__schemas.py
    |__books.py
    |__crud.py
```

2. Kerakli packagelarni o'rnatamiz

```shell
pip install fastapi uvicorn sqlalchemy
```

3. database.py fayl yaratib uni ichiga quyidagi kodlarni yozamiz

```shell
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"
# SQLALCHEMY_DATABASE_URL = "postgresql://user:password@postgresserver/db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL, connect_args={"check_same_thread": False}
)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

Yuqoridagi kod tahlili:
Bu qatorlar sizning ma'lumotlar bazasiga ulanish uchun kerakli obyektlarni yaratadi. Quyidagi ishlar bajariladi:

SQLALCHEMY_DATABASE_URL: Ma'lumotlar bazasiga ulanish uchun URL. Siz bu URLni o'zgartirib, foydalanishni istagan
ma'lumotlar bazasiga ulash uchun istalgan qo'llab-quvvatlovchiga (masalan, SQLite, PostgreSQL) ulanishingiz mumkin.

1. engine: Ma'lumotlar bazasiga ulanish uchun o'zgaruvchini yaratish. Ulanish sozlamalarini (masalan,
   check_same_thread=False SQLite da yoki ulanish parolini PostgreSQL da) ta'riflash mumkin.

2. SessionLocal: Ma'lumotlar bazasi sessiyalarini yaratish uchun o'zgaruvchi. Ma'lumotlarni yozish va o'qish
   operatsiyalari
   sessiya orqali bajariladi.

3. Base: Ma'lumotlar bazasidagi modellarni avtomatik ravishda yaratish uchun bazaviy model. Bu asosiy sinflarni
   ta'riflash
   uchun ishlatiladi. Bu obyekt sizning ma'lumotlar bazasiga ma'lumotlar qo'shish uchun asosiy sinflarni ta'riflashda
   foydalaniladi.


4. models.py nomli fayl yaratib uni ichida quyidagi kodlarni yozamiz.

```shell
from sqlalchemy import Boolean, Column, ForeignKey, Integer, String
from sqlalchemy.orm import relationship

from .database import Base


class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    email = Column(String, unique=True, index=True)
    hashed_password = Column(String)
    is_active = Column(Boolean, default=True)

    items = relationship("Item", back_populates="owner")


class Item(Base):
    __tablename__ = "items"

    id = Column(Integer, primary_key=True)
    title = Column(String, index=True)
    description = Column(String, index=True)
    owner_id = Column(Integer, ForeignKey("users.id"))

    owner = relationship("User", back_populates="items")
```

Yuqoridagi Kod tahlili:

Bu kod, SQLAlchemy modulini ishlatib, ma'lumotlar bazasidagi "users" va "items" jadvalidagi ma'lumotlar bilan ishlash
uchun modellarini aniqlaydi. Base obyekti, bu modellar uchun asosiy sinfni bildiradi.

1. User sinfi:

__tablename__: Ma'lumotlar bazasidagi "users" jadvalini belgilaydi.
id: Unikal foydalanuvchi identifikatori.
email: Foydalanuvchi elektron pochtasi, unikal va indekslangan.
hashed_password: Foydalanuvchi parolini hashlash.
is_active: Foydalanuvchi faol bo'lsa True, aks holda False.
items: Foydalanuvchi bilan bog'liq "Item" obyektlarini qaytaruvchi aloqa.

2. Item sinfi:

__tablename__: Ma'lumotlar bazasidagi "items" jadvalini belgilaydi.
id: Unikal element identifikatori.
title: Elementning nomi.
description: Element haqida tavsif.
owner_id: Elementni egasi bo'lgan foydalanuvchi identifikatori, "users" jadvalidan qisqa qatnashsa.
owner: Elementning egasi bo'lgan foydalanuvchi obyekti.

Bu kod foydalanuvchilar va ularning xoslagan mahsulotlari o'rtasidagi bog'lanishni ifodalaydi. Mahsulotlarga
foydalanuvchi ma'lumotlarini qo'shish uchun "User" obyektiga yordam beradi, va foydalanuvchi haqidagi ma'lumotlarga
mahsulotlarni qo'shish uchun "Item" obyektiga yordam beradi.

5. shemas.py faylini yaratib ichida quyidagi kodlarni yozamiz:

```shell
from pydantic import BaseModel


class ItemBase(BaseModel):
    title: str
    description: str | None = None


class ItemCreate(ItemBase):
    pass


class Item(ItemBase):
    id: int
    owner_id: int

    class Config:
        orm_mode = True


class UserBase(BaseModel):
    email: str


class UserCreate(UserBase):
    password: str


class User(UserBase):
    id: int
    is_active: bool
    items: list[Item] = []

    class Config:
        orm_mode = True
```

Yuqoridagi kod tahlili:

Bu kod Pydantic modellarini ta'riflaydi, ular ma'lumotlarni tahrirlash, ma'lumotlarni qo'shish va ulashning qulay
usullarini ta'minlaydi. Modellar quyidagicha ta'riflangan:

1. ItemBase sinfi:
title: Mahsulot nomi.
description: Mahsulot tavsifi. Bu maydon "None" qiymati bilan ta'riflanadi, ya'ni agar tavsif mavjud bo'lmasa ham,
qiymat "None" bo'ladi.

2. ItemCreate sinfi:
ItemBase sinfidan miras oladi. Bu sinfning ichida qo'shimcha xususiyatlar yo'q.

3. Item sinfi:
ItemBase sinfidan miras oladi.
id: Mahsulotning identifikatori.
owner_id: Mahsulot egasining identifikatori.
Config.orm_mode = True: Pydantic'da sinfi SQL Alchemy modeli sifatida ishlatish uchun belgilangan.

1. UserBase sinfi:
email: Foydalanuvchi email manzili.

2. UserCreate sinfi:
UserBase sinfidan miras oladi.
password: Foydalanuvchi paroli.

3. User sinfi:
UserBase sinfidan miras oladi.
id: Foydalanuvchi identifikatori.
is_active: Foydalanuvchi faolmi yoki yo'qmi.
items: Foydalanuvchining egasligidagi mahsulotlar ro'yxati. Bu ro'yxatni boshlang'ich qiymati bo'sh ro'yxatdir.

Bu modellar ko'rsatilgan ma'lumotlarni yaxshi ko'rish, tasdiqlash va qo'llashda qulayliklar yaratish uchun ishlatiladi.
orm_mode = True parametri esa Pydantic modellarni SQL Alchemy modellari sifatida ishlatish uchun kerakli ma'lumotlarni
olish uchun kerakli bo'lgan o'zgaruvchilar va metodlarini yo'qotishga yordam beradi.

6. crud.py faylini yaratamiz:
```shell
from sqlalchemy.orm import Session

from . import models, schemas


def get_user(db: Session, user_id: int):
    return db.query(models.User).filter(models.User.id == user_id).first()


def get_user_by_email(db: Session, email: str):
    return db.query(models.User).filter(models.User.email == email).first()


def get_users(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.User).offset(skip).limit(limit).all()


def create_user(db: Session, user: schemas.UserCreate):
    fake_hashed_password = user.password + "notreallyhashed"
    db_user = models.User(email=user.email, hashed_password=fake_hashed_password)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user


def get_items(db: Session, skip: int = 0, limit: int = 100):
    return db.query(models.Item).offset(skip).limit(limit).all()


def create_user_item(db: Session, item: schemas.ItemCreate, user_id: int):
    db_item = models.Item(**item.dict(), owner_id=user_id)
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item```
```

Yuqoridagi kod tahlili:

Bu kod ma'lumotlar bazasidan ma'lumotlarni olish, ma'lumotlarni qo'shish vaqti yordamida to'g'ri foydalanuvchini aniqlaydi. Kodning har bir funksiyasi quyidagi vazifalarni bajaradi:

get_user: Ma'lumotlar bazasidan foydalanuvchi obyektini olish.
get_user_by_email: Ma'lumotlar bazasidan e-mail bo'yicha foydalanuvchi obyektini olish.
get_users: Barcha foydalanuvchilarni olish.
create_user: Yangi foydalanuvchini yaratish.
get_items: Barcha mahsulotlarni olish.
create_user_item: Foydalanuvchiga oid mahsulotni yaratish.

Ushbu funktsiyalar Session obyektini qabul qilib, ma'lumotlar bazasi bilan bog'lanishni yuritadi. schemas modulidan kelgan Pydantic modellari ham ishlatiladi, shuningdek models modulidan kelgan SQLAlchemy modellari ham. Bu funktsiyalar ma'lumotlar bazasida qidiruv, qo'shish va boshqa amallarni bajarish uchun ishlatiladi.

7. books.py nomli fayl yaratamiz:
```shell
from fastapi import Depends, FastAPI, HTTPException
from sqlalchemy.orm import Session

from . import crud, models, schemas
from .database import SessionLocal, engine

models.Base.metadata.create_all(bind=engine)

app = FastAPI()


# Dependency
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()


@app.post("/users/", response_model=schemas.User)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
    db_user = crud.get_user_by_email(db, email=user.email)
    if db_user:
        raise HTTPException(status_code=400, detail="Email already registered")
    return crud.create_user(db=db, user=user)


@app.get("/users/", response_model=list[schemas.User])
def read_users(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    users = crud.get_users(db, skip=skip, limit=limit)
    return users


@app.get("/users/{user_id}", response_model=schemas.User)
def read_user(user_id: int, db: Session = Depends(get_db)):
    db_user = crud.get_user(db, user_id=user_id)
    if db_user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return db_user


@app.post("/users/{user_id}/items/", response_model=schemas.Item)
def create_item_for_user(
    user_id: int, item: schemas.ItemCreate, db: Session = Depends(get_db)
):
    return crud.create_user_item(db=db, item=item, user_id=user_id)


@app.get("/items/", response_model=list[schemas.Item])
def read_items(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    items = crud.get_items(db, skip=skip, limit=limit)
    return items
```

Yuqoridagi kod tahlili:

Bu FastAPI ilovalarini yaratish uchun kengaytirilgan koddan tashkil topgan bo'lib, qisqa ta'riflar quyidagicha:

create_user: Yangi foydalanuvchini ro'yxatga olish uchun POST endpointini yaratadi. Agar foydalanuvchi allaqachon ro'yxatdan o'tgan bo'lsa, xatolik beradi.
read_users: Barcha foydalanuvchilarni olish uchun GET endpointini yaratadi.
read_user: Bir foydalanuvchi ma'lumotlarini olish uchun GET endpointini yaratadi.
create_item_for_user: Foydalanuvchiga oid mahsulotni yaratish uchun POST endpointini yaratadi.
read_items: Barcha mahsulotlarni olish uchun GET endpointini yaratadi.

Bu kod FastAPI ilovalari bilan bog'liq har qanday ma'lumotni qabul qilish, saqlash, olish va uni ko'rsatish uchun mo'ljallangan. Bu ilovalar, ma'lumotlar bazasida foydalanuvchilar va mahsulotlarning ma'lumotlarini saqlash uchun ishlatiladi.

https://aminalaee.dev/sqladmin/
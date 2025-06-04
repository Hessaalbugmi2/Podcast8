
# Podcast8
📊 **تقرير تحليل بيانات الاستماع للبودكاست**

---

### 🧾 **مقدمة**

في هذا التقرير، قمنا بتحليل بيانات الاستماع للبودكاست بهدف فهم سلوكيات المستمعين وتفضيلاتهم، وذلك من خلال دراسة ملفات المستخدمين، الحلقات، وسجلات الاستماع. تم استخدام أدوات تحليل البيانات لاستخلاص رؤى تساعد في تحسين المحتوى وزيادة التفاعل.

---

### 🛠️ **خطوات العمل**

1. **تحميل البيانات**:
```python
import pandas as pd

users_df = pd.read_csv("C:/Users/hessa/OneDrive/Documents/Hacker (1)_6/Desktop/8assignment/users.csv")
episodes_df = pd.read_csv("C:/Users/hessa/OneDrive/Documents/Hacker (1)_6/Desktop/8assignment/episodes.csv")
listens_df = pd.read_json("C:/Users/hessa/OneDrive/Documents/Hacker (1)_6/Desktop/8assignment/listens.json")
```

2. **تنظيف البيانات**:
```python
listens_df_cleaned = listens_df[listens_df["duration_seconds"] > 0]
```

3. **دمج البيانات**:
```python
merged_df = listens_df_cleaned.merge(episodes_df, on="episode_id", how="left")
merged_df = merged_df.merge(users_df, on="user_id", how="left")
```

4. **تحليل البيانات**:
```python
# أكثر فئات الحلقات استماعًا
top_categories = merged_df["category"].value_counts()

# الفرق في متوسط مدة الاستماع بين الذكور والإناث
avg_duration_by_gender = merged_df.groupby("gender")["duration_seconds"].mean()

# متوسط عدد الحلقات التي يستمع لها المستخدم الواحد
average_episodes = merged_df.groupby("user_id")["episode_id"].nunique().mean()
```

5. **إنشاء توصيات مخصصة**:
```python
def recommend_episodes(user_id: int, listens_df, episodes_df):
    user_listens = listens_df[listens_df["user_id"] == user_id]
    if user_listens.empty:
        return f"🚫 لا توجد استماعات للمستخدم {user_id}"
    
    top_categories = user_listens["category"].value_counts().index.tolist()
    listened_episode_ids = user_listens["episode_id"].unique()
    
    recommendations = episodes_df[episodes_df["category"].isin(top_categories)]
    recommendations = recommendations[~recommendations["episode_id"].isin(listened_episode_ids)]
    
    return recommendations.head(3)
```

6. **تصوير البيانات**: تم استخدام Power BI لرسم الرسوم البيانية الموضحة.

---

### 📈 **النتائج**

1. **أكثر فئات الحلقات استماعًا**:
   - الفئة الأكثر استماعًا هي `Society`، تليها `Sports` و `News`.

2. **متوسط مدة الاستماع حسب الجنس**:
   - الإناث استمعن لمدة أطول من الذكور في المتوسط.

3. **متوسط عدد الحلقات التي يستمع لها المستخدم الواحد**:
   - بلغ المتوسط **4.42 حلقة** لكل مستخدم.

---

### ✅ **التوصيات**

* التركيز على الفئات الأكثر شعبية مثل Society وSports.
* تحسين المحتوى المُقدم للذكور لزيادة مدة الاستماع.
* دعم الفئات التي تمتلك متوسط استماع أطول في التسويق وصنع القرار.
* الاستفادة من دالة التوصيات لتقديم محتوى ملائم لكل مستخدم.



---

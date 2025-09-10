# Database-Migration-Issue_Django

### Process-1:
- After any update of any Model
  ```
  python manage.py makemigrations
  ```
- then,
  ```
  python manage.py migrate
  ```
### Process-2:
- If shows `No Change Detected`
  ```
  python manage.py makemigrations cognitive_tests_
  ```
- Here, `cognitive_tests_` is django app. then,
  ```
  python manage.py migrate
  ```
- or,
  ```
  python manage.py migrate cognitive_tests_
  ```
### Process-3:
- If it still shows 'No Changes Detected'
   ```
  python manage.py makemigrations cognitive_tests_ --fake
  ```
- Here, `cognitive_tests_` is django app. then,
  ```
  python manage.py migrate --fake
  ```
- or,
  ```
  python manage.py migrate cognitive_tests_ --fake
  ```
- or,
  ```
  python manage.py migrate --fake-initial
  ```
### Process-4:
- If it still shows 'No Changes Detected'
- Force empty migrations
   ```
  python manage.py makemigrations cognitive_tests --empty --name recreate_missing_table
  ```
- Now, check the migration folder, You should now see output like:
  ```
  Migrations for 'cognitive_tests':
  cognitive_tests/migrations/000X_recreate_missing_table.py
  ```
- Manually create fake Migrations. Edit `000X_recreate_missing_table.py`
  ```
  import django.db.models.deletion
  from django.conf import settings
  from django.db import migrations, models
  
  class Migration(migrations.Migration):

    dependencies = [
        ('cognitive_tests_', '0001_initial'),
    ]

    operations = [
         migrations.CreateModel(
            name='MCHATRResult',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('username', models.CharField(blank=True, db_index=True, max_length=150, null=True)),
                ('email', models.EmailField(db_index=True, max_length=254)),
                ('answers', models.JSONField()),
                ('total_score', models.IntegerField()),
                ('datetime', models.CharField(max_length=200)),
                ('user', models.ForeignKey(on_delete=django.db.models.deletion.CASCADE, to=settings.AUTH_USER_MODEL)),
            ],
        ),
    ]

  ```
- Here, `cognitive_tests_` is django app. Save it.
- then migrate,
  ```
  python manage.py migrate
  ```
- or,
  ```
  python manage.py migrate cognitive_tests_
  ```
### Clear the migration history for each app
- Run the `showmigrations` command so we can keep track of what is going on:
  ```
  python manage.py showmigrations
  ```
[See more](https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html)

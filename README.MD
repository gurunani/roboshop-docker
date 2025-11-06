### Docker Build & Push Script

```bash
for i in $(ls -d */); do
  cd $i
  name=$(basename "$i")
  docker build -t 9eguru/$name:v1 .
  docker push 9eguru/$name:v1
  cd ..
done

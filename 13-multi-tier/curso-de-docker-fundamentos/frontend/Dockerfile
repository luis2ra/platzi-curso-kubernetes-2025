FROM nginx:latest

# Copy the site files to nginx html directory
COPY /sitio /usr/share/nginx/html

# Rename linktree.html to index.html so nginx serves it as the default page
RUN mv /usr/share/nginx/html/linktree.html /usr/share/nginx/html/index.html

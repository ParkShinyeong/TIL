//// -- LEVEL 1
//// -- Tables and References

// Creating tables
Table users as U {
  id int [pk, increment] // auto-increment
  user_name varchar
  profile_img image 
  post image
  follower varchar
  following varchar
  
}
  Ref: users.follower < follower.user_name


Table follower {
  id int [pk, increment]
  user_name varchar
  follower_number int
 }
 
Table following {
   id int [pk, increment]
   user_name varchar
   following_number int 
 }
 
 
Table Post {
 Post_id int [pk, increment]
 Post image
 Upload_date datetime
 }
 
Table hashtag{
  hashtag_id int [pk, increment]
  hashtag varchar
}

Table comment {
  comment_id int [pk, increment]
  comment varchar
  create_at datetime
}

Table like {
  like_id int
  like_number int 
  user_push_like varchar 
}


# Codtech-task1
-- Create User Node
CREATE TABLE Users (
    UserID INT IDENTITY(1,1) PRIMARY KEY,
    Username VARCHAR(50) NOT NULL,
    Email VARCHAR(100) NOT NULL
) AS NODE;

-- Create Post Node
CREATE TABLE Posts (
    PostID INT IDENTITY(1,1) PRIMARY KEY,
    Content TEXT NOT NULL,
    CreatedAt DATETIME DEFAULT GETDATE()
) AS NODE;
-- Create Follows Edge (User follows User)
CREATE TABLE Follows (
    FollowDate DATETIME DEFAULT GETDATE()
) AS EDGE;

-- Create Likes Edge (User likes Post)
CREATE TABLE Likes (
    LikeDate DATETIME DEFAULT GETDATE()
) AS EDGE;
-- 1. Insert Users
INSERT INTO Users (Username, Email) VALUES 
('alice_dev', 'alice@email.com'),
('bob_coder', 'bob@email.com'),
('charlie_ux', 'charlie@email.com');

-- 2. Insert Posts
INSERT INTO Posts (Content) VALUES 
('Building a social media app using SQL Graph!'),
('Graph databases make relationship querying so easy.');

-- 3. Insert Follows Edges
INSERT INTO Follows ($from_id, $to_id) VALUES 
((SELECT $node_id FROM Users WHERE Username = 'alice_dev'), (SELECT $node_id FROM Users WHERE Username = 'bob_coder')),
((SELECT $node_id FROM Users WHERE Username = 'bob_coder'), (SELECT $node_id FROM Users WHERE Username = 'charlie_ux')),
((SELECT $node_id FROM Users WHERE Username = 'charlie_ux'), (SELECT $node_id FROM Users WHERE Username = 'alice_dev'));

-- 4. Insert Likes Edges
INSERT INTO Likes ($from_id, $to_id) VALUES 
((SELECT $node_id FROM Users WHERE Username = 'bob_coder'), (SELECT $node_id FROM Posts WHERE PostID = 1)),
((SELECT $node_id FROM Users WHERE Username = 'charlie_ux'), (SELECT $node_id FROM Posts WHERE PostID = 1));
SELECT User2.Username AS Following
FROM Users User1, Follows, Users User2
WHERE MATCH(User1-(Follows)->User2)
  AND User1.Username = 'alice_dev';
**  Output**

bob_coder

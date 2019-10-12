
--------------------------------------------------------------------------
|             File Upload Using Multer in Node.js and Express             |
--------------------------------------------------------------------------

Step 1 : Install nodejs in your system / Make workspace directory / Run follwoing comment 

			mkdir file_upload
			cd  file_upload
			npm init --yes
		
Step 2 : Install Requred packages using NPM

			npm install --save express multer body-parser ejs
			npm install -g nodemon (optional - used to run app.js automatically while any file content changes)
		
Step 3 : Create app.js file and Add follwoing code in app.js
			
			touch app.js
		
			const path = require('path');
			const express = require('express');
			const ejs = require('ejs');
			const bodyParser = require('body-parser');
			
			const app = express();
			
			// Base index route
			app.get('/', function(req, res) {
				res.send('File Upload Using Multer in Node.js and Express');
			});

			// Server Listening
			app.listen(3000, () => {
				console.log('Server is running at port 3000');
			});
			
			nodemon app (OR) npm start
			
Setp 4 : Define view engin with ejs / public path / view files path / bodyParser
			
			//set views file
			app.set('views',path.join(__dirname,'views'));
			
			//set view engine
			app.set('view engine', 'ejs');
			app.use(bodyParser.json());
			app.use(bodyParser.urlencoded({ extended: false }));
			
Step 5 : Define and configure Multer Storage
			
			const multer = require('multer');
			
			// For Multer Storage
			var multerStorage = multer.diskStorage({
			  destination: function (req, file, callback) {
				callback(null, path.join(__dirname,'my_uploads'));
			  },
			  filename: function (req, file, callback) {
				callback(null, Date.now() + '_' + file.originalname);
			  }
			});
			
			// For Single File upload
			var multerSigleUpload = multer({ storage: multerStorage });
			
			// For Multiple File upload
			var multerMultipleUpload = multer({ storage: multerStorage }).array("multipleImage", 3);


Setp 6 : Define index path with '/' and HTML file
			
			//route for index page
			app.get('/',(req, res) => {
				res.render('file_upload_index', {
					title: 'File Upload Using Multer in Node.js and Express'
				});
			});
			
Step 7 : Define File Upload function in app.js
			
			//route for single fileupload
			app.post("/singleFile", multerSigleUpload.single('singleImage'), function(req, res) {
				const file = req.file
				if (!file) {
					return res.end("Please upload a file!");
				}
				res.redirect('/');
			});
			
			
			//route for multiple file upload
			app.post("/multipleFile", function(req, res) {
				multerMultipleUpload(req, res, function(err) {
					if (err) {
						return res.end("Files uploading unsucessfully!");
					}
					res.redirect('/');
				});
			});


Setp 8 : Run a server and check with Browser

			npm start (OR) nodemon app

			http://localhost:3000/
			
Step 9 : Define a image upload form with file input

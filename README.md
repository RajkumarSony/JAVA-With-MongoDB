# JAVA-With-MongoDB


import java.util.ArrayList;
import java.util.Iterator;
import org.bson.Document;
import com.mongodb.MongoClient;
import com.mongodb.MongoClientURI;
import com.mongodb.MongoCredential;
import com.mongodb.client.FindIterable;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoCursor;
import com.mongodb.client.MongoDatabase;
import com.mongodb.client.MongoIterable;
import com.mongodb.client.model.Filters;
import com.mongodb.client.model.Updates;

public class ConnectToDB { 
   
	public static void main( String args[] ) {  
		
		String database		=	"StudentDB";
		String tablename	=	"Student";
		
		String src = "mongodb+srv://RajkumarSony:8252444848@cluster1.q9ccf.mongodb.net/StudentDB?retryWrites=true&w=majority";
		
		try {
			MongoClientURI uri = new MongoClientURI(src);
			MongoClient mongoClient = new MongoClient(uri);
			
			
			//----------- Creating Credentials ------------------------------//
			MongoCredential credential; 
			credential = MongoCredential.createCredential( "RajkumarSony", database, "8252444848".toCharArray()); 
			System.out.println("Credentials :: "+ credential);
			System.out.println("Connected to the database successfully..."); 
			
			
			//----------- Accessing Database ------------------------------//
			MongoDatabase db = mongoClient.getDatabase( database ); 
			
			
	    	//----------- Creating Table / Collection ------------------------------//
	    	db.createCollection("sampleCollection5"); 
	        System.out.println("Collection created successfully..");
			
			
			//---------- Accessing  Table / Collection -------------------------//  
			MongoCollection<Document> table = db.getCollection( tablename );	//	table is also called collection.
			
			
			//---------- Creating Document ---------------------------//    
		    Document doc = new Document(); 
		    doc.append("id", "S006");
		    doc.append("name","Komal"); 
		    doc.append("usn","1CR19MCA05"); 
		    
		    
		    //----------- Inserting Data ------------------------------//  
		    table.insertOne(doc); 
		    System.out.println("Data has been inserted successfully..."); 
		    
		    
		    //----------- Reading Data ------------------------------//  
	    	try (MongoCursor<Document> cur = table.find().iterator()) {
	    	    while (cur.hasNext()) {
	    	        var document = cur.next();
	    	        var std = new ArrayList<>(document.values());
	    	        System.out.printf("%s :  %s%n", std.get(1), std.get(2));
	    	    }
	    	}
		
	    	
	    	//----------- Updating Data ------------------------------//  
	        table.updateOne(Filters.eq("id", "S006"), Updates.set("name", "Adil"));
	        table.updateOne(Filters.eq("id", "S006"), Updates.set("usn", "1CR19MCA12"));
	        System.out.println("Document update successfully...");
	        
	        
	        //----------- Printing Data After Update Operation ------------------------------//
	        FindIterable<Document> iterDoc = table.find();
	        Iterator it = iterDoc.iterator();
	        while (it.hasNext()) {
	        	System.out.println(it.next());
	        }
	    	
	        
	        //----------- Deleting Data ------------------------------//  
	        table.deleteOne(Filters.eq("name", "Lokendra"));
	        System.out.println("Document deleted successfully...");
	        
	        table.deleteMany(Filters.eq("name", "Komal"));
	        System.out.println("All Documents deleted successfully...");
	        
	        
	        //----------- Printing Data After Delete Operation ------------------------------//
	    	try (MongoCursor<Document> cur = table.find().iterator()) {
	    	    while (cur.hasNext()) {
	    	        var document = cur.next();
	    	        var std = new ArrayList<>(document.values());
	    	        System.out.printf("%s :  %s%n", std.get(1), std.get(2));
	    	    }
	    	}
	        
	        
	        //----------- Listing  Tables / Collections ------------------------------//
	        MongoIterable<String> list = db.listCollectionNames();
	        int count = 1;
	        System.out.println("List of collections:");
	        for (String name : list) {
	           System.out.println(count+".  "+name);
	           count++;
	        }
	        
	        
	        //----------- Deleting  Table / Collection ------------------------------//
	        db.getCollection("sampleCollection3").drop();
	        System.out.println("Collection dropped successfully...");
	        
	        
	        //----------- Listing Collections After Delete Operation ------------------------------//
	        System.out.println("List of collections after the delete operation:");
	        count = 1;
	        for (String name : list) {
	           System.out.println(count+".  "+name);
	           count++;
	        }
	    	
		}catch(Exception e) {
			System.out.println(e);
		}
    }  
}

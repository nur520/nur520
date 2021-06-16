package Twitter.test;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

import twitter4j.GeoLocation;
import twitter4j.Query;
import twitter4j.QueryResult;
import twitter4j.Status;
import twitter4j.Twitter;
import twitter4j.TwitterException;
import twitter4j.TwitterFactory;


public class twtr {

//クラスを作成
	public static void main(String[] args) throws TwitterException {
		
		//初期化
		Twitter twitter = new TwitterFactory().getInstance(); 
		Query query = new Query();
		
		try {
	
			File file = new File("Twitter.log");
				PrintWriter pw;
				pw = new PrintWriter(new BufferedWriter(new FileWriter(file,true)));
				
		query.setQuery("メッシ");   // 検索ワードをセット
		query.setLang("ja");      //日本語で検索
	 　 query.setCount(100);　　　//1度のリクエストで取得するTweetの数（100が最大）

	    for (int i = 1; i <= 15; i++); 　// 最大1500件,15回ループ
		QueryResult result = twitter.search(query);

		System.out.println("ヒット数 : " + result.getTweets().size());
		
		 
				//検索結果を見る
		for (Status tweet : result.getTweets()) {
			String Str = tweet.getText();
		     
		if(Str.indexOf("#") == -1 && Str.indexOf("http") == -1 
					&& Str.indexOf("RT") == -1 && Str.indexOf("@") == -1){
					pw.print("@"+ Str);
					}
		
		if(result.hasNext()){
				query = result.nextQuery();
				}else{
				break;
				}
		
		 
		 if(tweet.isRetweet()) {
			 GeoLocation location = tweet.getGeoLocation();tweet.getPlace();
		 if( location == null ){
			 }else{
				  System.out.println(location);
				}
		   
		    String url= "https://twitter.com/" + tweet.getUser().getScreenName() 
		     + "/status/" + tweet.getId();
		    
	        
			System.out.println("日時:"+tweet.getCreatedAt());
            System.out.println("ID:"+tweet.getId());
            System.out.println("本文:" + Str.replace("\\n", ""));
            System.out.println("RT数:"+tweet.getRetweetCount());
			System.out.println("RET数:" +  tweet.getRetweetCount() + " FAV数:" +  tweet.getFavoriteCount());
            System.out.println("Place:" + "_" );
          
            System.out.println("Twitter URL:" + url);
            
            pw.flush();
            
            pw.println("日時:"+tweet.getCreatedAt());
            pw.println("ID:"+tweet.getId());
            pw.println("本文:" + Str.replace("\\n", ""));
            pw.println("RT数:"+tweet.getRetweetCount());
			pw.println("RET数:" +  tweet.getRetweetCount() + " FAV数:" +  tweet.getFavoriteCount());
            pw.println("Place:" + "_" );
            pw.println("Twitter URL:" + url);
	    }
		}
		    pw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	
    }

}





		



			
		


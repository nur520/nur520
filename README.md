package Twitter.test;

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;

import twitter4j.GeoLocation;
import twitter4j.Query;
import twitter4j.QueryResult;
import twitter4j.Status;
import twitter4j.Twitter;
import twitter4j.TwitterException;
import twitter4j.TwitterFactory;
import twitter4j.URLEntity;


public class twtr {

//クラスを作成
	@SuppressWarnings("removal")
	public static void main(String[] args) throws TwitterException {
		
		//初期化
		Twitter twitter = new TwitterFactory().getInstance(); 
		Query query = new Query();
		@SuppressWarnings("unused")
		List<Status> statusList = twitter.getHomeTimeline();
		
		try {
			File file = new File( "YYYY年MM月DD日_Twitter.log");
			File fNew = new File("YYYYMMDD_Twitter.log");
				PrintWriter pw;
				pw = new PrintWriter(new BufferedWriter(new FileWriter(file,true)));
				if (file.exists()) {
		            //ファイル名変更実行
		            file.renameTo(fNew); 

		query.setQuery("東京五輪");   // 検索ワードをセット
		query.setLang("ja");        //日本語のTweetだけ検索
	        query.setCount(100);        // 1度のリクエストで取得するTweetの数（100が最大）
	        //query.setQuery("filter:media");
	    
	        for (int i = 1; i <= 15; i++) {     // 最大1500件（15ページ）なので15回ループ
	        query.setSinceId(i);
	        QueryResult result = twitter.search(query); // 検索実行

		System.out.println("ヒット数 : " + result.getTweets().size());
		System.out.println("ページ数 : " + new Integer(i).toString());
			
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
		  
            String url1= "https://twitter.com/" + tweet.getUser().getScreenName() + "/status/" + tweet.getId();
	        URLEntity[] urls = tweet.getURLEntities();
            for(URLEntity urlEntity : urls) {
            String url = urlEntity.getURL();
               
               
       		
	        System.out.println("日時:"+tweet.getCreatedAt());
            System.out.println("ID:"+tweet.getId());
            System.out.println("本文:" + Str.replace("\\n", ""));
            System.out.println("RT数:"+tweet.getRetweetCount());
	        System.out.println("RET数:" +  tweet.getRetweetCount() + " FAV数:" +  tweet.getFavoriteCount());
            System.out.println("Place:" + "_" );
            System.out.println("TWJ-Client-{0.0.0}" +url);
            System.out.println("Twitter URL:" + url1);
            
            pw.flush();
            
            pw.println("日時:"+tweet.getCreatedAt());
            pw.println("ID:"+tweet.getId());
            pw.println("本文:" + Str.replace("\\n", ""));
            pw.println("RT数:"+tweet.getRetweetCount());
	        pw.println("RET数:" +  tweet.getRetweetCount() + " FAV数:" +  tweet.getFavoriteCount());
            pw.println("Place:" + "_" );
            pw.println("TWJ-Client-{0.0.0}" +url);
            pw.println("Twitter URL:" + url1);
            }
            }
	    
		}
	    }
	    }
	    	pw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	
    }

}

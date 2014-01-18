To sign and send OAuth request using libOAuth from Apple
--------------------------------------------------------

Example, 

<code>
    OACredential *credential = [[objc_getClass("OACredential") alloc] init];

    [credential setConsumerSecret:@"TWITTER_SECRET"];
    [credential setConsumerKey:@"TWITTER_KEY"];
    [credential setOauthTokenSecret:@"TWITTER_TOKEN_SECRET"];
    [credential setOauthToken:@"TWITTER_TOKEN"];

    OAURLRequestSigner *signer = [[objc_getClass("OAURLRequestSigner") alloc] initWithCredential:credential];
    [signer setSignatureMethod:0];
    
    NSMutableURLRequest *request = [[NSMutableURLRequest alloc] initWithURL:[NSURL URLWithString:@"https://api.twitter.com/1.1/statuses/update.json"]];
    
    NSData *data = [@"status=thanks @moeseth" dataUsingEncoding:NSUTF8StringEncoding];
    [request setHTTPBody:data];
    [request setHTTPMethod:@"POST"];
    
    request = [signer signedURLRequestWithRequest:request];
    
    [NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
        NSLog(@"data -> %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);
    }];

</code>
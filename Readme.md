# Description

Enables following a series of redirects (shortened urls)

# Prerequisites

Tested on Ruby 1.8.7 and 1.9.3

# Example Code
  
	require 'unwind'
	
	follower = Unwind::RedirectFollower.new('http://j.mp/xZVND1')
	follower.resolve
	assert_equal 'http://ow.ly/i/s1O0', follower.final_url 
	assert_equal 'http://j.mp/xZVND1', follower.original_url
	assert_equal 2, follower.redirects.count
	
# Hat tip

Most of the code is based on John Nunemaker's blog post [Following Redirects with Net/HTTP](http://railstips.org/blog/archives/2009/03/04/following-redirects-with-nethttp/).

# Overriding Faraday requests

Faraday is used internally to make the requests.  
To get fine-grained access to the Faraday object pass in an optional block to the resolve method.

## Example that overrides timeouts and retries

    follower = Unwind::RedirectFollower.new("http://t.co/pae2zZmnJl")
    result = follower.resolve do |current_url, headers|
      conn = Faraday.new do |faraday|
        faraday.request :retry, 3
        faraday.adapter  Faraday.default_adapter
      end
      response = conn.get do |req|
        req.options[:timeout] = 5
        req.options[:open_timeout] = 3
        req.url my_url
      end
    end

# License 

Provided under the Do Whatever You Want With This Code License.

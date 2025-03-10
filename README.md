# Quick Query (alpha)

[![Build Status](https://travis-ci.org/pkarl/quick-query.svg?branch=master)](https://travis-ci.org/pkarl/quick-query)

Quick Query is an interface for fetching data from WordPress that's intuitive and pleasant to use. It uses a jQuery-like syntax to chain together simple pieces of information and get you the right information in return.

## How it works

Quick Query began as a scrap of syntactic sugar for dealing with [WP_Query](http://codex.wordpress.org/Class_Reference/WP_Query). Over the course of a few projects, this sugary treat grew into necessity that simplified 80% of the time-consuming query code into a few pleasant lines.

Here's an comparison in a demanding scenario:
```php
// The WP_Query way
$args = array(
	'post_type' => 'post',
	'tax_query' => array(
		'relation' => 'AND',
		array(
			'taxonomy' => 'movie_genre',
			'field' => 'slug',
			'terms' => array( 'action', 'comedy' )
		),
		array(
			'taxonomy' => 'actor',
			'field' => 'id',
			'terms' => array( 103, 115, 206 ),
			'operator' => 'NOT IN'
		)
	)
);
$query = new WP_Query( $args );

// The Quick Query way
$posts = $q->type('post')->tags( ['movie_genre' => ['action','comedy'], 'actor' => [103, 115, 206]], 'AND' );

```

## The plan

I'm working on creating an honest-to-goodness open source version of it from the ground up. At the time of this writing, Quick Query is just a class wrapper with all of our code commented out. 

I'll be working on this (along with anyone who enjoys the idea of mitigating the emotional damage WP_Query causes) to incrementally re-add the functionality of `QQuery` along with tests and docs as we go.

# TODO

- [x] Post Type
 * `$qq->type('some-type')`
- [x] Fetch Post by ID
 * `$qq->get(42)`
- [x] ACF sensory
- [x] Fetch Post in set of IDs
 * `$qq->in([2, 5, 42])`
- [x] Sort order
 * `$qq->sort('date', 'DESC')`
- [x] Exclude ID(s)
- [x] Return all posts
 * `$qq->all()`
- [x] Set posts per page
 * `$qq->ppp(20)`
- [ ] Meta Fields options
 * `$qq->meta('author')`
- [x] Pagination & Offset
 * `$qq->ppp(3)->page(2)`
- [x] Filter by post status
 * `$qq->status('published')`
 * `$qq->status(['published','draft'])`
- [ ] Terms, Taxonomies & Categories
 * `$qq->term('cats')`
 * `$qq->tax('animals')`
 * `$qq->tax(['animals'=>['cats', 'dogs'], ['pizza'=>['deep', 'NYC']], 'OR')`
 * `$qq->category('fish')`
- [ ] Authors
 * `$qq->author(1)`
 * `$qq->author('Pete Karl II')`
- [ ] Parents & Children
 * `$qq->parent(22)`
 * `$qq->parent('some-slug')`
 * `$qq->children('some_post_type')`

## Needs Tests

* ACF, see `tests/test_qq_acf.php`

## Needs docs

*(everything)*

# Running Tests

I recommend using 10up's [VVV](https://github.com/Varying-Vagrant-Vagrants/VVV) to set up a wordpress environment.

From there, what I've done is cloned this repo, added it as a sync'd folder in VVV's Vagrant file, and then symlinked it to my wordpress repos.

> To sync any additional folder, add `config.vm.synced_folder "/path/to/your/folder/", "/srv/vagrant/mirror/"` to your Vagrantfile so you can edit code in your OS and run tests VM effortlessly

To run the tests, I `vagrant ssh` into the vbox, navigate to the plugin directory, and run `phpunit` (or, more frequently, `phpunit --debug`)

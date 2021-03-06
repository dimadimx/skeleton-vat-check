# skeleton-vat-check

## Description

Check Vat number in 4 steps:
1. syntax via a predefined regex array (list available in config)
2. is european
3. caching table (prevent denial of VIES service)
4. VIES service

## Installation

Installation via composer:

    composer require tigron/skeleton-vat-check

## Howto setup

Run the initial migration or executed the following queries

	CREATE TABLE `vat_check_cache` (
  		`id` int(11) NOT NULL AUTO_INCREMENT,
  		`vat_number` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  		`country_id` int(11) NOT NULL,
  		`valid` int(11) NOT NULL DEFAULT '0',
  		`created` datetime NOT NULL,
  		PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;

	INSERT INTO `transaction` (`classname`, `created`, `scheduled_at`, `data`, `retry_attempt`, `recurring`, `completed`, `failed`, `locked`, `frozen`, `parallel`) 
	VALUES ('Cleanup_Vat_Check_Cache', now(), now(), '', '', '1', '0', '0', '0', '0', '0');
	
## Howto use

	/**
	 * the VAT number to check
	 */
	$vat_number = '0886776275';
	
	/**
	 * The Country object. 
	 * Your object should implement the \Skeleton\Vat\Check\Country Interface
	 */
	$country = $your_country_object;
	
	/**
	 * A string for the fail reason. This will be passed by reference and the 
	 * error can be retrieved after calling the validate method
	 */
	$reason = '';
	
	/** 
	 * A flag to ignore a cache lookup
	 */
	$ignore_cache = false;
	
	/**
	 * Perform the call
	 */
    \Skeleton\Vat\Check\Check::validate($vat_number, $country, $reason, $ignore_cache) 


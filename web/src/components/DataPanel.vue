<template>
	<div class="panel data-panel">
		<div class="data-title">{{ title }}</div>
		<div class="data-point">
			<div class="point-title">Earned Revenue</div>
			<div class="point-value" v-html="earnedRevenueSC" />
			<div class="point-value point-secondary" v-html="earnedRevenueCurrency" />
		</div>
		<div class="data-point">
			<div class="point-title">Potential Revenue</div>
			<div class="point-value" v-html="potentialRevenueSC" />
			<div class="point-value point-secondary" v-html="potentialRevenueCurrency" />
		</div>
		<div class="data-point">
			<div class="point-title">Burnt Collateral</div>
			<div class="point-value" v-html="burntCollateralSC" />
			<div class="point-value point-secondary" v-html="burntCollateralCurrency" />
		</div>
		<div class="data-point">
			<div class="point-title">{{ contractLabel }}</div>
			<div class="point-value" v-html="contractsStr" />
		</div>
		<div class="data-point">
			<div class="point-title">Successful Contracts</div>
			<div class="point-value" v-html="successfulContractsStr" />
		</div>
		<div class="data-point">
			<div class="point-title">Failed Contracts</div>
			<div class="point-value" v-html="failedContractsStr" />
		</div>
	</div>
</template>

<script>
import { mapState } from 'vuex';
import { formatPriceString, formatNumber } from '@/utils/format';
import BigNumber from 'bignumber.js';

export default {
	props: {
		title: String,
		earnedRevenue: String,
		potentialRevenue: String,
		burntCollateral: String,
		contractLabel: String,
		contracts: Number,
		successfulContracts: Number,
		failedContracts: Number
	},
	computed: {
		...mapState(['exchangeRateSC', 'currency']),
		earnedRevenueSC() {
			let val = new BigNumber(this.earnedRevenue);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		potentialRevenueSC() {
			let val = new BigNumber(this.potentialRevenue);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		burntCollateralSC() {
			let val = new BigNumber(this.burntCollateral);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		earnedRevenueCurrency() {
			let val = new BigNumber(this.earnedRevenue);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val, 2, this.currency, this.exchangeRateSC[this.currency]);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		burntCollateralCurrency() {
			let val = new BigNumber(this.burntCollateral);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val, 2, this.currency, this.exchangeRateSC[this.currency]);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		potentialRevenueCurrency() {
			let val = new BigNumber(this.potentialRevenue);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			const format = formatPriceString(val, 2, this.currency, this.exchangeRateSC[this.currency]);

			return `${format.value} <span class="currency-display">${format.label}</span>`;
		},
		contractsStr() {
			let val = new BigNumber(this.contracts);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			return `${formatNumber(val)}`;
		},
		successfulContractsStr() {
			let val = new BigNumber(this.successfulContracts);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			return `${formatNumber(val)}`;
		},
		failedContractsStr() {
			let val = new BigNumber(this.failedContracts);

			if (!val.isFinite() || val.isNaN())
				val = new BigNumber(0);

			return `${formatNumber(val)}`;
		}
	}
};
</script>

<style lang="stylus" scoped>
.data-panel {
	display: grid;
	grid-template-columns: minmax(0, 1fr);
	padding: 15px;
	grid-gap: 15px;
}

.data-title, .point-title, .point-value {
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}

.data-title {
	grid-column: 1 / -1;
	text-align: center;
	color: rgba(255, 255, 255, 0.84);
}

.point-title {
	margin-bottom: 5px;
	color: rgba(255, 255, 255, 0.54);
	font-size: 0.9rem;
	text-align: right;
}

.point-value {
	color: primary;
	font-size: 1.1rem;
	text-align: right;
}

.point-secondary {
	color: rgba(255, 255, 255, 0.54);
	font-size: 1rem;
	margin-top: 2px;
}
</style>